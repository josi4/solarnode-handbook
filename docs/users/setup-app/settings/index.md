# Settings

The Settings page in SolarNode Setup is where you can configure all available SolarNode settings.
The page is divided 4 main sections, outlined next.

## Components

The Components section lists all the configurable _multi-instance components_ available on
your SolarNode. _Multi-instance_ means you can configure any number of a given component,
each with their own settings.

For example imagine you want to collect data from a power meter, solar inverter, and weather
station, all of which use the Modbus protocol. To do that you would configure three _instances_ of
the Modbus Device component, one for each device.

Use the **Manage** button for any listed compoennt to [add or remove instances](manage-component.md)
of that component.

An **instance count badge** appears next to any component with at least one instance configured.

<figure markdown>
  ![Settings components list](../../../images/users/setup/setup-components%402x.png){width=740}
</figure>

## Settings

Other configurable services that are not [Components](#components) appear in the Settings section.

Each setting will include a :fontawesome-regular-circle-question: button that will show you a
brief description of that setting.

<figure markdown>
  ![Setting tool tips have helpful information](../../../images/users/setup/setup-settings-toolip%402x.png){width=790}
</figure>

After making any change, an **Active value** label will appear, showing the currently active value
for that setting.

<figure markdown>
  ![Modified settings show the previous value](../../../images/users/setup/setup-setting-changed-value%402x.png){width=566}
</figure>

In order to save your changes, you must click the **Save All Changes** button. You may need to scroll
the page to find it!

<figure markdown>
  ![Save all changes button](../../../images/users/setup/setup-save-changes%402x.png){width=734}
</figure>

## Backup & Restore

The Backup & Restore section lets you manage SolarNode backups. Each backup contains a snapshot
of the settings you have configured, the node's certificate, and custom plugins.

<figure markdown>
  ![Backup and restore form](../../../images/users/setup/setup-backups%402x.png){width=650}
</figure>

### File System Backup Service

The File System Backup Service is the default Backup Service provided by SolarNode. It saves
the backup onto the node itself. In order to be able to restore your settings if the node is damaged
or lost, you must download a copy of a backup using the **Download** button, and save the file
to a safe place.

!!! warning

    If you do not download a copy of a backup, you run the risk of losing your settings and
    node certificate, making it impossible to restore the node in the event of a catastrophic
    hardware failure.

The configurable settings of the File System Backup Service are:

| Setting | Description |
|:--------|:------------|
| Backup Directory | The folder (on the node) where the backups will be saved. |
| Copies | The number of backup copies to keep, before deleting the oldest backup. |

### S3 Backup Service

The S3 Backup Service creates cloud-based backups in [AWS S3][s3] (or any compatible provider). You
must configure the credentials and S3 location details to use before any backups can be created.

<figure markdown>
  ![S3 Backup settings form](../../../images/users/setup/setup-s3-backup%402x.png){width=460}
</figure>

!!! note

    The S3 Backup Service requires the [S3 Backup Service Plugin][s3-backup-plugin].

The configurable settings of the S3 Backup Service are:

| Setting | Description |
|:--------|:------------|
| AWS Token | The AWS access token to authenticate with. |
| AWS Secret | The AWS access token secret to authenticate with. |
| AWS Region | The name of the Amazon region to use, for example `us-west-2`. |
| S3 Bucket | The name of the S3 bucket to use. |
| S3 Path | An optional root path to use for all backup data (typically a folder location). |
| Storage Class | A supported storage class, such as STANDARD (the default), `STANDARD_IA`, `INTELLIGENT_TIERING`, `REDUCED_REDUNDANCY`, and so on. |
| Copies | The number of backup copies to keep, before deleting the oldest backup. |
| Cache Seconds | The amount of time to cache backup metadata such as the list of available backups, in seconds. |

## Settings Backup & Restore

The Settings Backup & Restore section provides a way to manage [Settings Files][settings-file]
and Settings Resources, both of which are backups for the configured settings in SolarNode.

!!! warning

    Settings Files and Settings Resources do **not** include the node's certificate or custom
    plugins. See the [Backup & Restore](#backup-restore) section for managing "full" backups
    that _do_ include those items.

<figure markdown>
  ![Settings import/export form](../../../images/users/setup/setup-settings-backup%402x.png){width=676}
</figure>

The **Export** button allows you to download a [Settings File][settings-file] with the currently active configuration.

The **Import** button allows you to upload a previously-downloaded [Settings File][settings-file].

The **Settings Resource** menu and associated **Export to file** button allow you to download
specialized settings files, offered by some components in SolarNode.

The **Auto backups** area will have a list of buttons, each of which will let you download a
[Settings File][settings-file] that SolarNode automatically created. Each button shows you
the date the settings backup was created.

[settings-file]: ../../settings.md
[s3]: https://aws.amazon.com/s3/
[s3-backup-plugin]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.backup.s3
