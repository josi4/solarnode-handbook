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
| Service Name       | A unique ID for the filter, to be referenced by other components. |
| Service Group      | An optional service group name to assign. |
| Source ID          | A [regular expression][regex] to match the input source ID(s) to filter. |
| Required Mode      | If configured, an [operational mode](https://github.com/SolarNetwork/solarnetwork/wiki/SolarNode-Operational-Modes) that must be active for this filter to be applied. |
| Required Tag       | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
| Limit Seconds      | A throttle limit, in seconds, to apply to matching datum. |

## Settings notes

 * **Source ID** — This is a case-insensitive [regular expression][regex] to match against
   datum source ID values. If omitted then datum for _all_ source ID values will be filtered,
   otherwise only datum with _matching_ source ID values will be filtered.
 * **Limit Seconds** — The throttle limit is applied to datum by source ID. Before each datum is
	uploaded to SolarNetwork, the filter will check how long has elapsed since a datum with the same
	source ID was uploaded. If the elapsed time is less than the configured limit, the datum will
	not be uploaded.

[opmodes]: ../op-modes.md
[placeholders]: ../placeholders.md
[regex]: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html
[sdf]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/
[src]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/README-Throttle.md
