# Backup Manager

The [`net.solarnetwork.node.backup.BackupManager`][BackupManager] API provides SolarNode with a
modular backup system composed of [Backup Services](#backup-service) that provide storage for backup
data and [Backup Resource Providers](#backup-resource-provider) that contribute data to be backed up
and support restoring backed up data.

The Backup Manager coordinates the creation and restoration of backups, delegating most of its
functionality to the _active_ [Backup Service](#backup-service). The active Backup Service can be
controlled through [configuration](#configuration).

The Backup Manager also supports _exporting_ and _importing_ [Backup Archives](#backup-archive),
which are just `.zip` archives using a defined folder structure to preserve all backup resources
within a single backup.

This design of the SolarNode backup system makes it easy for SolarNode plugins to contribute
resources to backups, without needing to know where or how the backup data is ultimately stored.

!!! info "What goes in a Backup?"

	In SolarNode a Backup will contain all the critical settings that are unique to that node, such as:

	1. The node's security certificate
	2. SolarNetwork association details
	3. User accounts
	4. Settings
	5. _and more_

## Configuration

The Backup Manager can be configured under the `net.solarnetwork.node.backup.DefaultBackupManager`
[configuration namespace](../../users/configuration.md):

<div markdown="1" class="props-explicit-col-widths">

| Key | Default | Description |
|:----|:--------|:------------|
| `backupRestoreDelaySeconds` | 15 | A number of seconds to delay the attempt of restoring a backup, when a backup has been previously marked for restoration. This delay gives the platform time to boot up and register the [backup resource providers](#backup-resource-provider) and other services required to perform the restore. |
| `preferredBackupServiceKey` | net.solarnetwork.node.backup.FileSystemBackupService | The key of the preferred (active) [Backup Service](#backup-service) to use. |

</div>

## Backup

The [`net.solarnetwork.node.backup.Backup`][Backup] API defines a unique backup, created by a
[Backup Service](#backup-service). Backups are uniquely identified with a unique **key** assigned by
the [Backup Service](#backup-service) that creates them.

A `Backup` does not itself provide access to any of the resources associated with the backup.
Instead, the `getBackupResources()` method of [`BackupService`][BackupService] returns them.

## Backup Archive

The [Backup Manager](#backup-manager) supports _exporting_ and _importing_ specially formed `.zip`
archives that contain a complete [Backup](#backup). These archives are a convenient way to transfer
settings from one node to another, and can be used to restore SolarNode on a new device.

## Backup Resource

The [`net.solarnetwork.node.backup.BackupResource`][BackupResource] API defines a unique item
within a [Backup](#backup). A Backup Resource could be a file, a database table, or anything that
can be serialized to a stream of bytes. Backup Resources are both provided by, and restored with,
[Backup Resource Providers](#backup-resource-provider) so it is up to the Provider implementation to
know how to generate and then restore the Resources it manages.

## Backup Resource Provider

The [`net.solarnetwork.node.backup.BackupResourceProvider`][BackupResourceProvider] API defines a
service that can both generate and restore [Backup Resources](#backup-resource). Each implementation
is identified by a unique **key**, typically the fully-qualified Java class name of the
implementation.

When a Backup is **created**, all Backup Resource Provider services registered in SolarNode will be
asked to contribute their Backup Resources, using the `getBackupResources()` method.

When a Backup is **restored**, Backup Resources will be passed to their associated Provider with the
`restoreBackupResource(BackupResource)` method.

## Backup Service

The [`net.solarnetwork.node.backup.BackupService`][BackupService] API defines the bulk of the
SolarNode backup system. Each implementation is identified by a unique **key**, typically the
fully-qualified Java class name of the implementation.

To **create** a Backup, use the `performBackup(Iterable<BackupResource>)` method, passing in the
collection of [Backup Resources](#backup-resource) to include.

To **list** the available Backups, use the `getAvailableBackups(Backup)` method.

To **view** a single Backup, use the `backupForKey(String)` method.

To **list the resources** in a Backup, use the `getBackupResources(Backup)`method.

### FileSystemBackupService

SolarNode provides the `net.solarnetwork.node.backup.FileSystemBackupService` default Backup Service
implementation that saves [Backup Archives](#backup-archive) to the node's own file system.

### S3BackupService

The [`net.solarnetwork.node.backup.s3`][s3-backup-plugin] plugin provides the
`net.solarnetwork.node.backup.s3.S3BackupService` Backup Service implementation that saves all
Backup data to [AWS S3][s3].

[s3]: https://aws.amazon.com/s3/
[s3-backup-plugin]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.backup.s3
[Backup]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/backup/Backup.html
[BackupManager]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/backup/BackupManager.html
[BackupResource]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/backup/BackupResource.html
[BackupResourceProvider]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/backup/BackupResourceProvider.html
[BackupService]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/backup/BackupService.html
