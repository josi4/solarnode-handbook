# Placeholders

SolarNode supports _placeholders_ in some setting values, such as datum data source IDs. These allow
you to define a set of parameters that can be consistently applied to many settings.

For example, imagine you manage many SolarNode devices across different buildings or sites, You'd
like to follow a naming convention for your datum data source ID values that include a code for the
building the node is deployed in, along the lines of `/BUILDING/DEVICE`. You could define a
placeholder `building` and then configure the source IDs like `/{building}/device`. On each node
you'd define the `building` placeholder with a building-specific value, so at runtime the nodes
would resolve actual source ID values with those names replacing the `{building}` placeholder.

## Placeholder syntax

Placeholders are written using the form `{name:default}` where `name` is the placeholder name and
`default` is an optional default value to apply if no placeholder value exists for the given name.
If a default value is not needed, omit the `colon` so the placeholder becomes just `{name}`.

For example, imagine a set of placeholder values like

| Name     | Value   |
|:---------|:--------|
| building | OFFICE1 |
| room     | BREAK   |

Here are some example settings with placeholders with what they would resolve to:

| Input                          | Resolved value         |
|:-------------------------------|:-----------------------|
| `/{building}/meter`            | `/OFFICE1/meter`       |
| `/{building}/{room}/temp`      | `/OFFICE1/BREAK/temp`  |
| `/{building}/{floor:1}/{room}` | `/OFFICE1/1/BREAK`     |
