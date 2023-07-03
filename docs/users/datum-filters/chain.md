# Filter Chain

The **Datum Filter Chain** is a [User Datum Filter][udf] that you configure with a list, or chain,
of _other_ User Datum Filters. When the Filter Chain executes, it executes each of the configured
Datum Filters, in the order defined.  This filter can be used like any other Datum Filter, allowing
multiple filters to be applied in a defined order.

<figure markdown>
  ![Datum Filter Chain diagram](../../images/users/datum-filters/datum-filter-chain.svg#only-light){width=480}
  ![Datum Filter Chain diagram](../../images/users/datum-filters/datum-filter-chain.dark.svg#only-dark){width=480}
  <caption>A Filter Chain acts like an ordered group of Datum Filters</caption>
</figure>

!!! tip

	Some services support configuring only a **single** Datum Filter setting. You can use a Filter
	Chain to apply multiple filters in those services.

## Settings

<figure markdown>
  ![Filter Chain component settings](../../images/users/datum-filters/datum-filter-chain-settings%402x.png){width=736 loading=lazy}
</figure>

Each filter configuration contains the following overall settings:

| Setting | Description |
|:--------|:------------|
| Available Filters | A read-only list of **Service Name** values of [User Datum Filter][udf] components that have been configured. You can copy any value from this list and paste it into the **Datum Filters** list to include that filter in the chain. |
| Service Name      | A unique ID for the filter, to be referenced by other components. |
| Service Group     | An optional service group name to assign. |
| Required Mode     | If configured, an [operational mode][opmodes] that must be active for this filter to be applied. |
| Required Tag       | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
| Datum Filters     | The list of **Service Name** values of [User Datum Filter][udf] components to apply to datum. |

[opmodes]: ../op-modes.md
[udf]: ../setup-app/settings/datum-filters.md#user-datum-filters
