# Package Maintenance

SolarNodeOS supports a [wide variety of software packages][snos]. You can install new packages as
well as apply package updates as they become available. The [apt][apt] command performs these tasks.

## Update package metadata

For SolarNodeOS to know what packages, or package updates, are available, you need to periodically
update the available package information. This is done with the `apt update` command:

```sh title="Update package information"
sudo apt update # (1)!
```

1. The `sudo` command runs other commands with [administrative privledges](index.md#administrator-access).
   It will prompt you for your user account password (typically the `solar` user).

## List installed packages

Use the `apt list` command to list the installed packages:

```sh title="Listing the installed packages"
apt list --installed
```

## Update packages

To see if there are any package updates available, run `apt list` like this:

```sh title="List packages with updates available"
apt list --upgradable
```

If there are updates available, that will show them. You can apply **all** package updates with the
`apt upgrade` command, like this:

```sh title="Upgrade all packages"
sudo apt upgrade
```

If you want to install an update for a specific package, use the [`apt install`](#install-packages)
command instead.

!!! tip

	The `apt upgrade` command will update existing packages and install packages that are required
	by those packages, but it will never remove an existing package. Sometimes you will want to
	allow packages to be removed during the upgrade process; to do that use the `apt full-upgrade`
	command.

## Search for packages

Use the `apt search` command to search for packages. By default this will match package names and
their descriptions. You can search just for package names by including a `--names-only` argument.

```sh title="Search for packages"
# search for "name" across package names and descriptions
apt search name

# search for "name" across package names only
apt search --names-only name

# multiple search terms are logically "and"-ed together
apt search name1 name2
```

## Install packages

The `apt install` command will install an available package, or an individual package update.

```sh title="Install package"
sudo apt install mypackage
```

## Remove packages

You can remove packages with the `apt remove` command. That command will preserve any system
configuration associated with the package(s); if you would like to also remove that you can
use the `apt purge` command.

```sh title="Removing packages"
sudo apt remove mypackage

# use `purge` to also remove configuration
sudo apt purge mypackage
```

[apt]: https://manpages.debian.org/bullseye/apt/apt.8.en.html
[snos]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarNodeOS
