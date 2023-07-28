---
title: Parameter Expression
---
# Parameter Expression Datum Filter

The [Parameter Expression Datum Filter][src] provides a way to generate filter parameters by
evaluating expressions against existing properties. The generated parameters will be available to
any further datum filters in the same filter chain.

!!! tip

	Parameters are useful as temporary variables that you want to use during datum processing but do
	not want to include as datum properties that get posted to SolarNet.

This filter is provided by the [Standard Datum Filters][sdf] plugin.

## Settings

<figure markdown>
  ![Parameter Expression filter component settings](../../images/users/datum-filters/parameter-filter-settings%402x.png){width=629 loading=lazy}
</figure>

Each filter configuration contains the following overall settings:

| Setting            | Description                                                       |
|:-------------------|:------------------------------------------------------------------|
| Service Name       | A unique ID for the filter, to be referenced by other components. |
| Service Group      | An optional service group name to assign. |
| Source ID          | A case-insensitive [pattern][regex] to match the input source ID(s) to filter. If omitted then datum for _all_ source ID values will be filtered, otherwise only datum with _matching_ source ID values will be filtered. |
| Required Mode      | If configured, an [operational mode][opmodes] that must be active for this filter to be applied. |
| Required Tag       | Only apply the filter on datum with the given tag. A tag may be prefixed with `!` to invert the logic so that the filter only applies to datum **without** the given tag. Multiple tags can be defined using a `,` delimiter, in which case **at least one** of the configured tags must match to apply the filter. |
| Expressions        |  A list of expression configurations that are evaluated to derive parameter values from other property values. |

Use the <kbd>+</kbd> and <kbd>-</kbd> buttons to add/remove expression configurations.

## Expression settings

Each expression configuration contains the following settings:

| Setting             | Description                                                       |
|:--------------------|:------------------------------------------------------------------|
| Parameter           | The filter parameter name to store the expression result in. |
| Expression          | The expression to evaluate. See [below](#expressions) for more info. |
| Expression Language | The [expression language][expr-lang] to write **Expression** in. |

## Expressions

See the [Expressions](../expressions.md) section for general expressions reference. This filter
supports Datum Expressions that lets you treat all datum properties, and filter parameters, as
expression variables directly.

[opmodes]: ../op-modes.md
[placeholders]: ../placeholders.md
[regex]: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html
[sdf]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/
[src]: https://github.com/SolarNetwork/solarnetwork-node/blob/develop/net.solarnetwork.node.datum.filter.standard/README-Parameter.md

