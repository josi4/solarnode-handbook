# Placeholders

SolarNode supports _placeholders_ in some setting values, such as datum data source IDs. These allow
you to define a set of parameters that can be consistently applied to many settings.

For example, imagine you manage many SolarNode devices across different buildings or sites. You'd
like to follow a naming convention for your datum data source ID values that include a code for the
building the node is deployed in, along the lines of `/BUILDING/DEVICE`. You could define a
placeholder `building` and then configure the source IDs like `/{building}/device`. On each node
you'd define the `building` placeholder with a building-specific value, so at runtime the nodes
would resolve actual source ID values with those names replacing the `{building}` placeholder,
for example `/OFFICE1/meter`.

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

## Static placeholder configuration

SolarNode will look for placeholder values defined in properties files stored in the
`conf/placeholders.d` directory by default. In SolarNodeOS this is the
`/etc/solarnode/placeholders.d` directory.

!!! warning

	These files are **only loaded once, when SolarNode starts up**. If you make changes to any of
	them then SolarNode must be restarted.

The properties file names must have a `.properties` extension and follow Java [properties
file][props-file] syntax. Put simply, each file contains lines like

```
name = value
```

where `name` is the placeholder name and `value` is its associated value. The example set of
placeholder values [shown previously](#placeholder-syntax) could be defined in a
`/etc/solarnode/placeholders.d/mynode.properties` file with this content:

```
building = OFFICE1
room = BREAK
```

# Dynamic placeholder configuration

SolarNode also supports storing placeholder values as [Settings](settings.md) using the key
`placeholder`. The SolarUser [/instruction/add][instr-add] API can be used with the
[UpdateSetting][UpdateSetting] topic to modify the placeholder values as needed. The `type` value is
the placeholder name and the `value` the placeholder value. Placeholders defined this way have
priority over any similarly-named placeholders defined statically. **Changes take effect as soon as
SolarNode receives and processes the instruction.**

!!! warning

	Once a placeholder value is set via the `UpdateSetting` instruction, the same value defined as a
	[static](#static-placeholder-configuration) placeholder will be overridden and changes to the
	static value will be ignored.

For example, to set the `floor` placeholder to `2` on node 123, you could make a `POST` request to
`/solaruser/api/v1/sec/instr/add/UpdateSetting` with the following JSON body:

```json
{
  "nodeId": 123,
  "params":{
    "key":   "placeholder",
    "type":  "floor",
    "value": "2"
  }
}
```

Multiple settings can be updated as well, using a different syntax. Here's a request that sets
both `floor` to `2` and `room` to `MEET`:

```json
{"nodeId":123,"parameters":[
  {"name":"key",   "value":"placeholder"},
  {"name":"type",  "value":"floor"},
  {"name":"value", "value":"2"},
  {"name":"key",   "value":"placeholder"},
  {"name":"type",  "value":"room"},
  {"name":"value", "value":"MEET"}
]}
```

[instr-add]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-API#queue-instruction
[props-file]: https://en.wikipedia.org/wiki/.properties
[UpdateSetting]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-API-enumerated-types#updatesetting
