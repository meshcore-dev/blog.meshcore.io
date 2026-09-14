---
layout: post
date: 2026-09-14 12:00:00 +1200
title: "Sensor Node Intro"
description: "How to get started programming your own MeshCore sensor nodes."
image: "assets/images/2026/09/14/sensor_mesh.jpg"
author: scottpowell
---
The MeshCore sensor node type is much harder to get going with as we don't
pre-build variants with each release, like we do with repeater, companion and room server,
so it requires some developer knowledge.

The reason behind this is because there simply are too many customisations
that typically would be required to accommodate, and there would have to be
thousands of variants. So, sensor nodes need to be designed for whatever purpose
they're intended for (eg. monitoring water level in a water tank)

## Setup

You will need to install [VSCode](https://code.visualstudio.com/download) and once that is installed, within VSCode install the
[PlatformIO](https://platformio.org/) extension.

Then use Git to clone the MeshCore firmware repo:

```git clone https://github.com/meshcore-dev/MeshCore.git```

## Starting with the example

The sensor firmware is essentially a starter or 'template' firmware, which has
most of the components you're going to need. In the project tree, it is in:

```
/examples/simple_sensor
```

Typical customisations should only need to touch the ```main.cpp``` module. This
has a section which defines a ```MyMesh``` class, and this inherits from ```SensorMesh``` 
which provides all the _plumbing_ and hooks which you can utilise.

## Choose a board variant

A lot of the boards supported should already have a PlatformIO target ```env``` defined
for the simple_sensor example firmware, like ```[env:Heltec_v3_sensor]```. But, you
may need to add your own ```env``` definition in a ```platformio.ini``` file in one of
the ```/variant``` folders.

This sets up the build rules/dependencies, etc for that board+firmware role. You may
need to modify the ```target.h/cpp``` files of the variant. These target modules 
assemble various global objects, like this:

```cpp
EnvironmentSensorManager sensors;
```

This one is a kind of _Swiss Army knife_ helper class which the other firmwares also
use (eg. repeater) for doing the low-level work of managing physical sensors, like
BME180's, etc. So, most of that is done for you, but you typically have to ENABLE which modules
your node is expecting to use. These can be enabled in your /variant ```platformio.ini``` file like this:

```
build_flags =
  -D ENV_INCLUDE_GPS=1
  -D ENV_INCLUDE_AHTX0=1
  -D ENV_INCLUDE_BME280=1
  -D ENV_INCLUDE_BMP280=1
  -D ENV_INCLUDE_SHTC3=1
  -D ENV_INCLUDE_SHT4X=1
  -D ENV_INCLUDE_LPS22HB=1
  -D ENV_INCLUDE_INA3221=1
  -D ENV_INCLUDE_INA219=1
  -D ENV_INCLUDE_INA226=1
  -D ENV_INCLUDE_INA260=1
  -D ENV_INCLUDE_MLX90614=1
  -D ENV_INCLUDE_VL53L0X=1
  -D ENV_INCLUDE_BME680=1
  -D ENV_INCLUDE_BMP085=1
```


## Basic concepts

The sensor node can utilise these features:
* Alerts (with High or Low priority)
* Telemetry queries (ie. other nodes can _pull_ telemetry from this node)
* Time series data
* Custom CLI command logic (eg. 'turn switch A on')
* Telemetry _push_ subscriptions (NEW: will be supported soon)

## Alerts

In the example this is demonstrated with the ```Trigger``` class and ```alertIf()``` calls:

```cpp
  Trigger low_batt, critical_batt;

  void onSensorDataRead() override {
    float batt_voltage = getVoltage(TELEM_CHANNEL_SELF);

    alertIf(batt_voltage < 3.4f, critical_batt, HIGH_PRI_ALERT, "Battery is critical!");
    alertIf(batt_voltage < 3.6f, low_batt, LOW_PRI_ALERT, "Battery is low");
  }
```
High priority alerts will re-try sending to nodes in the ACL which have
the ```PERM_RECV_ALERTS_HI``` bit set. The sensor nodes will wait for an ACK, like 
any other text message.

Low priority alerts only fire off the message _once_ and don't wait for an ACK. These are
sent to nodes in ACL with the ```PERM_RECV_ALERTS_LO``` bit set.

## Telemetry

This is basically _automatic_ as it's a core mechanism in MeshCore. The target/variant 
```sensor``` object will handle LPP encoding all the collected telemetry
readings.

Normally, other nodes to need to request telemetry from your sensor node, and a 
telemetry response is then sent, but very soon you will also be able to _subscribe_
to Telemetry _push_, where the sensor node sends telemetry packets to subscriber(s)
but only when certain values _change_ by some minimum amount. eg. a subscriber can
send SUBSCRIBE request, and specify that temperature must change by 2 Celcius.

Telemetry push will have some guard rails to hopefully minimise abuse:
* subscription has a timeout (returned in the subscribe response), so subscribers will need to re-subscribe. 
* must use a region scope (as fallback, if direct path is not established)
* subscriber must specify which LPP channels/types and _min deltas_ that these must change by to trigger push

## Time series data

The sensor can collect periodic readings, and stores these (in volatile memory, in a circular buffer)
using the ```TimeSeriesData``` helper class. Example:

```cpp
  TimeSeriesData  battery_data;

  MyMesh(mesh::MainBoard& board, mesh::Radio& radio, mesh::MillisecondClock& ms, mesh::RNG& rng, mesh::RTCClock& rtc, mesh::MeshTables& tables)
     : SensorMesh(board, radio, ms, rng, rtc, tables), 
       battery_data(12*24, 5*60)    // 24 hours worth of battery data, every 5 minutes
  {
  }

  void onSensorDataRead() override {
    float batt_voltage = getVoltage(TELEM_CHANNEL_SELF);

    battery_data.recordData(getRTCClock(), batt_voltage);   // record battery
  }

  int querySeriesData(uint32_t start_secs_ago, uint32_t end_secs_ago, MinMaxAvg dest[], int max_num) override {
    battery_data.calcMinMaxAvg(getRTCClock(), start_secs_ago, end_secs_ago, &dest[0], TELEM_CHANNEL_SELF, LPP_VOLTAGE);
    return 1;
  }
```

Remote nodes can then send 'min/max/average' queries to the sensor node, specifying
a time range. To achieve this, the ```querySeriesData()``` method must be overridden
as above. The return value is the number of data series supported by this node.

## Custom CLI Commands

Remote nodes can also enable actuators of different types, like turning an
LED on/off, moving a servo, etc. by adding custom CLI command logic. This is
done in the ```handleCustomCommand()``` method:

```cpp
  bool handleCustomCommand(uint32_t sender_timestamp, char* command, char* reply) override {
    if (strcmp(command, "magic") == 0) {    // example 'custom' command handling
      strcpy(reply, "**Magic now done**");
      return true;   // handled
    }
    return false;  // not handled
  }
```

This should be fairly straight-forward to adapt to whatever your sensor node
wants to do. Just return ```true``` to indicate the command was intercepted and
handled, and place the CLI response  in the ```reply``` buffer (which is sent to command sender node).
