# System Administration

SolarNode runs on SolarNodeOS, a Debian Linux-based operating system. If you are already
familiar with Debian Linux, or one of the other Linux distributions built from Debian
like Ubuntu Linux, you will find it pretty easy to get around in SolarNodeOS.

## System User Account

SolarNodeOS ships with a `solar` user account that you can use to log into the operating system.
The default password is `solar` but may have been changed by a system administrator.

!!! warning

	The `solar` user account is not related to the account you log into the SolarNode Setup App with.

The `solar` user can also become the `root` administrative user by way of the `sudo` command:

```sh title="Gain system administrative privledges with sudo"
$ sudo su -
```

You can also execute arbitrary commands directly, for example:

```sh title="Run a command as a system administrator"
$ sudo reboot
```

## Network Access with SSH

SolarNodeOS comes with an SSH service active, which allows you to remotely connect and
access the command line, using any [SSH client][ssh-clients].

[ssh-clients]: https://en.wikipedia.org/wiki/Comparison_of_SSH_clients
