# Developer Guide

This section of the handbook is geared towards developers working with the SolarNode codebase to
develop a [plugin](osgi/index.md).

## SolarNode source

The core SolarNode platform code is [available on GitHub][repo].

## Getting started

See the [SolarNode Development Guide][dev-guide] to set up your own development environment for
writing SolarNode plugins.

## SolarNode debugging

You can enable Java remote debugging for SolarNode on a node device for SolarNode plugin development
or troubleshooting by modifying the [SolarNode service
environment](../users/sysadmin/solarnode-service.md#solarnode-service-environment). Once enabled,
you can use SSH port forwarding to enable Java remote debugging in your Java IDE of choice.

To enable Java remote debugging, copy the `/etc/solarnode/env.conf.example` file to
`/etc/solarnode/env.conf`. The example already includes this support, using port `9142` for the
debugging port. Then restart the `solarnode` service:

```sh title="Creating a custom SolarNode environment with debugging support"
$ cp /etc/solarnode/env.conf.example /etc/solarnode/env.conf
$ sn-restart
```

Then you can use `ssh` from your development machine to forward a local port to the node's `9142`
port, and then have your favorite IDE establish a remote debugging connection on your local port.

For example, on a Linux or macOS machine you could forward port `8000` to a node's port `9142` like this:

```sh title="Creating a port-forwarding SSH connection from a development machine to SolarNode"
$ ssh -L8000:localhost:9142 solar@solarnode
```

Once that `ssh` connection is established, your IDE can be used to connect to `localhost:8000` for a
remote Java debugging session.


[dev-guide]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarNode-Development-Guide
[repo]: https://github.com/SolarNetwork/solarnetwork-node
