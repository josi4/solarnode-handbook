# Logging

Logging in SolarNode is configured in the `/etc/solarnode/log4j2.xml` file, which is in the [log4j
configuration format][log4j2-xml-conf]. The [default configuration][default-conf] in SolarNodeOS
sets the overall verbosity to `INFO` and logs to a temporary storage area
`/run/solarnode/log/solarnode.log`.

## Logging concepts

Log messages have the following general properties:

| Component | Example | Description |
|:----------|:--------|:------------|
| Timestamp | `2022-03-15 09:05:37,029` | The date/time the message was generated. Note the format of the timestamp depends on the logging configuration; the SolarNode default is shown in this example. |
| Level     | `INFO` | The severity/verbosity of the message (as determined by the developer). This is an enumeration, and from least-to-most severe: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`.  The level of a given logger allows messages with that level _or higher_ to be logged, while lower levels are skipped. The default SolarNode configuration sets the overal level to `INFO`, so `TRACE` and `DEBUG` messages are not logged. |
| Logger    | `ModbusDatumDataSource` | A category or namespace associated with the message. Most commonly these equate to Java class names, but can be any value and is determined by the developer. Periods in the logger name act as a delimiter, forming a hierarchy that can be tuned to log at different levels. For example, given the default `INFO` level, configuring the `net.solarnetwork.node.io.modbus` logger to `DEBUG` would turn on debug-level logging for all loggers in the Modbus IO namespace. Note that the default SolarNode configuration logs just a fixed number of the last characters of the logger name. This can be changed in the configuration to log more (or all) of the name, as desired. |
| Message | `Error reading from device.` | The message itself, determined by the developer. |
| Exception |  | Some messages include an exception stack trace, which shows the runtime call tree where the exception occurred. |

## Logger namespaces

The **Logger** component outlined in the previous section allows a lot of flexibility to configure
what gets logged in SolarNode. Setting the level on a given namespace impacts that namespace as well
as all namespaces _beneath_ it, meaning all other loggers that share the same namespace prefix.

For example, imagine the following two loggers exist in SolarNode:

 * `net.solarnetwork.node.io.modbus.serial.SerialModbusNetwork`
 * `net.solarnetwork.node.io.modbus.util.ModbusUtils`

Given the default configuration sets the default level to `INFO`, we can turn in `DEBUG` logging for
both of these by adding a `<Logger>` line like the following within the `<Loggers>` element:

```xml
<Logger name="net.solarnetwork.node.io.modbus" level="debug"/>
```

That turns on `DEBUG` for both loggers because they are both children of the `net.solarnetwork.node.io.modbus` namespace.
We could turn on `TRACE` logging for one of them like this:

```xml
<Logger name="net.solarnetwork.node.io.modbus"        level="debug"/>
<Logger name="net.solarnetwork.node.io.modbus.serial" level="trace"/>
```

That would also turn on `TRACE` for any other loggers in the `net.solarnetwork.node.io.modbus.serial`
namespace. You can limit the configuration all the way down to a full logger name if you like, for
example:

```xml
<Logger name="net.solarnetwork.node.io.modbus"                            level="debug"/>
<Logger name="net.solarnetwork.node.io.modbus.serial.SerialModbusNetwork" level="trace"/>
```

## Logging UI

The SolarNode UI supports configuring logger levels dynamically, without having to change the
logging configuration file. See the [Setup App / Settings / Logging](setup-app/settings/logging.md)
page for more information.

## Storage constraints

The default SolarNode configuration automatically rotates log files based on size, and limits the
number of historic log files kept around, to that its associated storage space is not filled up.
When a log file reaches the file limit, it is renamed to include a `-i.log` suffix, where `i` is an
offset from the current log. The default configuration sets the maximum log size to **1 MB** and
limits the number of historic files to **3**.

You can also adjust how much history is saved by tweaking the `<SizeBasedTriggeringPolicy>` and
`<DefaultRolloverStrategy>` configuration. For example to change to a limit of **9** historic files of
at most **5 MB** each, the configuration would look like this:

```xml
<Policies>
  <SizeBasedTriggeringPolicy size="5 MB"/>
</Policies>
<DefaultRolloverStrategy max="9"/>
```

## Persistent logging

By default SolarNode logs to temporary (RAM) storage that is discarded when the node reboots. The
configuration can be changed so that logs are written directly to persistent storage if you would
like to have the logs persisted across reboots, or would like to preserve more log history than can
be stored in the temporary storage area.

To make this change, update the `<RollingFile>` element's `fileName` and/or `filePattern` attributes to point to a persistent filesystem. SolarNode already has write permission to the `/var/lib/solarnode/var` directory, so an
easy location to use is `/var/lib/solarnode/var/log`, like this:

```xml
<RollingFile name="File"
    immediateFlush="false"
    fileName="/var/lib/solarnode/var/log/solarnode.log"
    filePattern="/var/lib/solarnode/var/log/solarnode-%i.log">
```

!!! warning

	This configuration can add a lot of stress to the node's storage medium, and may shorten its useful life.
	Consumer-grade SD cards in particular can fail quickly if SolarNode is writting a lot of information,
	such as verbose logging. Use of this configuration should be used with caution.

## Logging example: split across multiple files

Sometimes it can be useful to turn on verbose logging for some area of SolarNode, but have those
messages go to a different file so they don't clog up the main `solarnode.log` file. This can be
done by configuring additional _appender_ configurations.

The following example logging configuration creates the following log files:

 * `/var/log/solarnode/solarnode.log` - the main log
 * `/var/log/solarnode/filter.log` - filter logging
 * `/var/log/solarnode/mqtt-solarin.log` - MQTT wire logging to SolarIn
 * `/var/log/solarnode/mqtt-solarflux.log` - MQTT wire logging to SolarFlux

First you must create the `/var/log/solarnode` directory and give SolarNode permission to write there:

```sh
sudo mkdir /var/log/solarnode
sudo chgrp solar /var/log/solarnode
sudo chmod g+w /var/log/solarnode
```

Then edit the `/etc/solarnode/log4j2.xml` file to hold the following (adjust according to your
needs):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <RollingFile name="File"
        immediateFlush="true"
        fileName="/var/log/solarnode/solarnode.log"
        filePattern="/var/log/solarnode/solarnode-%i.log"><!-- (1)! -->
      <PatternLayout pattern="%d{DEFAULT} %-5p %40.40c; %msg%n"/>
      <Policies>
        <SizeBasedTriggeringPolicy size="5 MB"/>
      </Policies>
      <DefaultRolloverStrategy max="9"/>
    </RollingFile>
    <RollingFile name="Filter"
        immediateFlush="false"
        fileName="/var/log/solarnode/filter.log"
        filePattern="/var/log/solarnode/filter-%i.log"><!-- (2)! -->
      <PatternLayout pattern="%d{DEFAULT} %-5p %40.40c; %msg%n"/>
      <Policies>
        <SizeBasedTriggeringPolicy size="10 MB"/>
      </Policies>
      <DefaultRolloverStrategy max="9"/>
    </RollingFile>
    <RollingFile name="MQTT"
        immediateFlush="false"
        fileName="/var/log/solarnode/mqtt.log"
        filePattern="/var/log/solarnode/mqtt-%i.log"><!-- (3)! -->
      <PatternLayout pattern="%d{DEFAULT} %-5p %40.40c; %msg%n"/>
      <Policies>
        <SizeBasedTriggeringPolicy size="10 MB"/>
      </Policies>
      <DefaultRolloverStrategy max="9"/>
    </RollingFile>
    <RollingFile name="Flux"
        immediateFlush="false"
        fileName="/var/log/solarnode/flux.log"
        filePattern="/var/log/solarnode/flux-%i.log"><!-- (4)! -->
      <PatternLayout pattern="%d{DEFAULT} %-5p %40.40c; %msg%n"/>
      <Policies>
        <SizeBasedTriggeringPolicy size="10 MB"/>
      </Policies>
      <DefaultRolloverStrategy max="9"/>
    </RollingFile>
  </Appenders>
  <Loggers>
    <Logger name="org.eclipse.gemini.blueprint.blueprint.container.support" level="warn"/>
    <Logger name="org.eclipse.gemini.blueprint.context.support" level="warn"/>
    <Logger name="org.eclipse.gemini.blueprint.service.importer.support" level="warn"/>
    <Logger name="org.springframework.beans.factory" level="warn"/>

    <Logger name="net.solarnetwork.node.datum.filter" level="trace" additivity="false">
      <AppenderRef ref="Filter"/><!-- (5)! -->
    </Logger>

    <Logger name="net.solarnetwork.mqtt.queue" level="trace" additivity="false">
      <AppenderRef ref="MQTT"/>
    </Logger>

    <Logger name="net.solarnetwork.mqtt.influx" level="trace" additivity="false">
      <AppenderRef ref="Flux"/>
    </Logger>

    <Root level="info">
      <AppenderRef ref="File"/><!-- (6)! -->
    </Root>
  </Loggers>
</Configuration>
```

1. The `File` appender is the "main" application log where most logs should go.
2. The `Filter` appender is where we want `net.solarnetwork.node.datum.filter` messages to go.
3. The `MQTT` appender is where we want `net.solarnetwork.mqtt.queue` messages to go.
4. The `Flux` appender is where we want `net.solarnetwork.mqtt.influx` messages to go.
5. Here we include `additivity="false"` and add the `<AppenderRef>` element that refereneces
   the specific appender name we want the log messages to go to. The `additivity=false` attribute
   means the log messages will **only** go to the `Filter` appender, instead of _also_ going to
   the root-level `File` appender.
6. The **root-level** appender is the "default" destination for log messages, unless overridden
   by a specific appender like we did for the `Filter`, `MQTT`, and `Flux` appenders above.

### Separate file configuration notes

The various `<AppenderRef>` elements configure the appender name to write the messages to.

The various `additivity="false"` attributes disable appender _additivity_ which means the log
message will only be written to one appender, instead of being written to **all**
configured appenders in the hierarchy (for example the root-level appender).

The `immediateFlush="false"` turns on buffered logging, which means log messages are buffered in RAM
before being flushed to disk. This is more forgiving to the disk, at the expense of a delay before
the messages appear.

## Enable MQTT wire logging

MQTT wire logging means the raw MQTT packets send and received over MQTT connections will be logged
in an easy-to-read but very verbose format. For the MQTT wire logging to be enabled, it must be
activated with a special configuration file. Create the
`/etc/solarnode/services/net.solarnetwork.common.mqtt.netty.cfg` file with this content:

```
wireLogging = true
```

### MQTT wire log namespace

MQTT wire logs use a namespace prefix `net.solarnetwork.mqtt.` followed by the connection's host
name or IP address and port. For example SolarIn messages would use
`net.solarnetwork.mqtt.queue.solarnetwork.net:8883` and SolarFlux messages would use
`net.solarnetwork.mqtt.influx.solarnetwork.net:8884`.

[default-conf]: https://github.com/SolarNetwork/solarnode-os-packages/blob/develop/solarnode-base/debian/usr/share/solarnode/conf/log4j2-example.xml
[log4j2-xml-conf]: https://logging.apache.org/log4j/2.x/manual/configuration.html#configuration-with-xml
