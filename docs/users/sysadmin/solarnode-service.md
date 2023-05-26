# SolarNode Service

SolarNode is managed as a systemd service. There are some shortcut commands to more easily
manage the service.

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

### Enabling Java remote debugging

To enable Java remote debugging for SolarNode plugin development or troubleshooting, copy the
`/etc/solarnode/env.conf.example` file to `/etc/solarnode/env.conf`. The example already includes
this support, using port `9142` for the debugging port. Then restart the `solarnode` service:

```sh title="Creating a custom SolarNode environment with debugging support"
$ cp /etc/solarnode/env.conf.example /etc/solarnode/env.conf
$ sn-restart
```

Then you can use `ssh` from your development machine to port-forward a local port to the node's
`9142` port, and then have your favorite IDE establish a remote debugging connection on your local
port.

For example, on a Linux or macOS machine I could forward port `8000` to a node's port `9142` like this:

```sh title="Creating a port-forwarding SSH connection from a development machine to SolarNode"
$ ssh -L8000:localhost:9142 solar@solarnode
```

Once that `ssh` connection is established, your IDE can be used to connect to `localhost:8000` for a
Java debugging session.
