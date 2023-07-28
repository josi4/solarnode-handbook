---
title: Throttle
---
# Throttle Datum Filter

The [Throttle Datum Filter][src] provides a way to throttle **entire datum** over time, so that they
are posted to SolarNetwork less frequently than a plugin that collects the data produces them. This
can be useful if you need a plugin to collect data at a high frequency for use internally by
SolarNode but don't need to save such high resolution of data in SolarNetwork. For example, a plugin
that monitors a device and responds quickly to changes in the data might be configured to sample
data every second, but you only want to capture that data once per minute in SolarNetwork.

The general idea for filtering datum is to configure rules that define which datum **sources** you
want to filter, along with **time limit** to throttle matching datum by. Any datum matching the
sources that are captured faster than the time limit will filtered and **not** uploaded to
SolarNetwork.

This filter is provided by the [Standard Datum Filters][sdf] plugin.

## Settings

<figure markdown>
  ![Trottle Datum filter component settings](../../images/users/datum-filters/throttle-filter-settings@2x.png){width=600 loading=lazy}
</figure>

Each filter configuration contains the following overall settings:

| Setting            | Description |
|:-------------------|:------------|
--8<-- "snippets/users/datum-filters/base-filter-settings.md"
| Limit Seconds      | A throttle limit, in seconds, to apply to matching datum. The throttle limit is applied to datum by source ID. Before each datum is uploaded to SolarNetwork, the filter will check how long has elapsed since a datum with the same source ID was uploaded. If the elapsed time is less than the configured limit, the datum will not be uploaded. |

--8<-- "snippets/users/datum-filters/base-filter-settings-links.md"
[placeholders]: ../placeholders.md
[sdf]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/
[src]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/README-Throttle.md
