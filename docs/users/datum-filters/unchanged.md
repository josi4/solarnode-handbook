---
title: Unchanged
---
# Unchanged Datum Filter

The [Unchanged Datum Filter][src] provides a way to discard **entire datum** that have not changed
within a datum stream.

This filter is provided by the [Standard Datum Filters][sdf] plugin.

!!! tip

	See the [Unchanged Property Filter](./unchanged-property.md) for a filter that can discard
	individual unchanging _properties_ within a datum stream.

## Settings

<figure markdown>
  ![Unchanged Datum filter component settings](../../images/users/datum-filters/unchanged-filter-settings@2x.png){width=700 loading=lazy}
</figure>

Each filter configuration contains the following overall settings:

| Setting            | Description |
|:-------------------|:------------|
| Service Name       | A unique ID for the filter, to be referenced by other components. |
| Service Group      | An optional service group name to assign. |
| Source ID          | A [regular expression][regex] to match the input source ID(s) to filter. |
| Required Mode      | If configured, an [operational mode](https://github.com/SolarNetwork/solarnetwork/wiki/SolarNode-Operational-Modes) that must be active for this filter to be applied. |
| Required Tag       | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
| Unchanged Max Seconds | When greater than `0` then the maximum number of seconds to refrain from publishing an unchanged datum within a single datum stream. |

## Settings notes

 * **Source ID** — This is a case-insensitive [regular expression][regex] to match against
   datum source ID values. If omitted then datum for _all_ source ID values will be filtered,
   otherwise only datum with _matching_ source ID values will be filtered.
 * **Unchanged Max Seconds** — Use this setting to ensure a datum is included
   occasionally, even if the datum properties have not changed. Having at least one value per
   hour in a datum stream is recommended. This time period is always relative to the last
   unfiltered property within a given datum stream seen by the filter.

[opmodes]: ../op-modes.md
[placeholders]: ../placeholders.md
[regex]: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html
[sdf]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/
[src]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/README-Unchanged.md
