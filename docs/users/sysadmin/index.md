# System Administration

SolarNode runs on SolarNodeOS, a Debian Linux-based operating system. If you are already
familiar with Debian Linux, or one of the other Linux distributions built from Debian
like Ubuntu Linux, you will find it pretty easy to get around in SolarNodeOS.

## System User Account

SolarNodeOS ships with a `solar` user account that you can use to log into the operating system.
The default password is `solar` but may have been changed by a system administrator.

!!! warning

	The `solar` user account is not related to the account you log into the SolarNode
	[Setup App](../setup-app/index.md) with.

### Change system user account password

To change the system user account's password, use the `passwd` command.

```sh title="Changing the system user account password"
$ passwd
Changing password for solar.
Current password:
New password:
Retype new password:
passwd: password updated successfully
```

!!! tip

	Changing the `solar` user's password is **highly recommended** when you first deploy a node.

## Administrator Access

Some commands require administrative permission. The `solar` user can execute arbitrary commands
with administrative permission by prefixing the command with `sudo`. For example the `reboot` command will
reboot SolarNodeOS, but requires administrative permission.

```sh title="Run a command as a system administrator"
$ sudo reboot
```

The `sudo` command will prompt you for the `solar` user's password and then execute the given command
as the administrator user `root`.

The `solar` user can also become the `root` administrator user by way of the `su` command:

```sh title="Gain system administrative privledges with su"
$ sudo su -
```

Once you have become the `root` user you no longer need to use the `sudo` command, as you
already have administrative permissions.

## Network Access with SSH

SolarNodeOS comes with an SSH service active, which allows you to remotely connect and
access the command line, using any [SSH client][ssh-clients].

[ssh-clients]: https://en.wikipedia.org/wiki/Comparison_of_SSH_clients
