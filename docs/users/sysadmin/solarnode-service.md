# SolarNode Service

SolarNode is managed as a [systemd][systemd-man] service. There are some shortcut commands to more
easily manage the service.

| Command | Description |
|:--------|:------------|
| `sn-start` | Start the SolarNode service. |
| `sn-restart` | Restart the SolarNode service. |
| `sn-status` | View status information about the SolarNode service (see if it is running or not). |
| `sn-stop` | Stop the SolarNode service. |

The `sn-stop` command requires administrative permissions, so you may be prompted for your system
account password (usually the `solar` user's password).

## SolarNode service environment

You can modify the environment variables passed to the SolarNode service, as well as modify
the Java runtime options used. You may want to do this, for example, to turn on Java
remote debugging support or to give the SolarNode process more memory.

The systemd `solarnode.service` unit will load the `/etc/solarnode/env.conf` environment
configuration file if it is present. You can define arbitrary environment variables using
a simple `key=value` syntax.

SolarNodeOS ships with a `/etc/solarnode/env.conf.example` file you can use for reference.

[systemd-man]: https://manpages.debian.org/bullseye/systemd/systemd.1.en.html
