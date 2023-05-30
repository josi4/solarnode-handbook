---
title: Downsample
---
# Downsample Datum Filter

The [Downsample Datum Filter][src] provides a way to down-sample higher-frequency datum samples into
lower-frequency (averaged) datum samples. The filter will collect a configurable number of samples
and then generate a down-sampled sample where an average of each collected instantaneous property is
included. In addition minimum and maximum values of each averaged property are added.

This filter is provided by the [Standard Datum Filters][sdf] plugin.

## Settings

<figure markdown>
  ![Downsample filter component settings](../../images/users/datum-filters/downsample-filter-settings%402x.png){width=600 loading=lazy}
</figure>

| Setting            | Description                                                       |
|:-------------------|:------------------------------------------------------------------|
| Service Name          | A unique ID for the filter, to be referenced by other components. |
| Service Group         | An optional service group name to assign. |
| Source ID             | The source ID(s) to filter. |
| Required Mode         | If configured, an [operational mode](../op-modes.md) that must be active for this filter to be applied. |
| Required Tag          | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
| Sample Count          | The number of samples to average over. |
| Decimal Scale         | A maximum number of digits after the decimal point to round to. Set to`0` to round to whole numbers. |
| Property Excludes     | A list of property names to exclude. |
| Min Property Template | A string format to use for computed minimum property values. Use `%s` as the placeholder for the original property name, e.g. `%s_min`. |
| Max Property Template | A string format to use for computed maximum property values. Use `%s` as the placeholder for the original property name, e.g. `%s_max`. |


[sdf]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/
[src]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/README-Downsample.md
