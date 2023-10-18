# Datum

In SolarNetwork a **datum** is the fundamental time-stamped data structure collected by SolarNodes
and stored in SolarNet. It is a collection of _properties_ associated with a specific _information
source_ at a specific _time_.

!!! note "Example plain language description of a datum"

	the **temperature** and **humidity** collected from **my weather station** at **1 Jan 2023 11:00 UTC**

In this example datum description, we have all the components of a datum:

| Datum component | Description |
|:----------------|:------------|
| node            | the (implied) node that collected the data |
| properties      | **temperature** and **humidity** |
| source          | **my weather station** |
| time            | **1 Jan 2023 11:00 UTC** |

A **datum stream** is the collection of datum from **a single node** for **a single source** over time.

A **datum object** is modeled as a flexible structure with the following core elements:

| Element     | Type   | Description |
|:------------|:-------|:------------|
| `nodeId`    | number | A unique ID assigned to nodes by SolarNetwork. |
| `sourceId`  | string | A node-unique identifier that defines a single stream of data from a specific source, up to 64 characters long. Certain characters are not allowed, see [below](#datum-source-ids). |
| `created`   | date   | A time stamp of when the datum was collected, or the date the datum is associated with. |
| `samples`   | [datum samples](#datum-samples) | The collected properties. |

A datum is uniquely identified by the three combined properties (`nodeId`, `sourceId`, `created`).

## Datum source IDs

Source IDs are user-defined strings used to distinguish between different information sources within
a single node. For example, a node might collect data from an energy meter on source ID `Meter` and
a solar inverter on `Solar`. SolarNetwork does not place any restrictions on source ID values, other
than a 64-character limit. However, there is are some conventions used within SolarNetwork that are
useful to follow, especially for larger deployment of nodes with many source IDs:

 * Keep IDs as short as, for example `Meter1` is better than `Schneider ION6200 Meter - Main Building`.
 * Use a path-like structure to encode a logical hierarchy, in least specific to most specific
   order. For example `/S1/B1/M1` could imply the first meter in the first building on the first site.
 * The `+` and `#` characters **should not be used**. This is actually a constraint in the MQTT
   protocol used in parts of SolarNetwork, where the MQTT topic name includes the source ID. These
   characters are MQTT topic filter wildcards, and cannot be used in topic names.
 * Avoid using [wildcard][wildcard-pats] special characters.

The path-like structure becomes useful in places where [wildcard patterns][wildcard-pats] are used,
like [security policies][sec-policy] or [datum queries][datum-list]. It is generally worthwhile
spending some time planning on a source ID taxonomy to use when starting a new project with
SolarNetwork.

## Datum samples

The properties included in a datum object are known as **datum samples**. The samples are modeled as
a collection of named properties, for example the **temperature** and **humidity** properties in
the earlier example datum could be represented like this:

```json title="Example representation of datum samples from a weather station source"
{
	"temperature" : 21.5,
	"humidity"    : 68
}
```

Another datum samples acquired from a power meter might look like this:

```json title="Example representation of datum samples from a power meter source"
{
	"watts"     : 2150,
	"wattHours" : 6834834349,
	"mode"      : "auto"
}
```

## Datum property classifications

The datum samples are actually further organized into three _classifications_:

| Classification | Key | Description |
|:---------------|:----|:------------|
| **instantaneous** | `i` | a single reading, observation, or measurement that does not accumulate over time |
| **accumulating**  | `a` | a reading that accumulates over time, like a meter or odometer |
| **status**        | `s` | non-numeric data, like staus codes or error messages |

These classifications help SolarNetwork understand how to aggregate the datum samples over time.
When SolarNode uploads a datum to SolarNetwork, the sample will include the classification of each property.
The previous example would thus more accurately be represented like this:

```json title="Example representation of datum samples with classifications"
{
  "i": {
    "watts"     : 2150 // (1)!
  },
  "a": {
    "wattHours" : 6834834349 // (2)!
  },
  "s": {
    "mode"      : "auto" // (3)!
  }
}
```

1. `watts` is an **instantaneous** measurement of power that does not accumulate
2. `wattHours` is an **accumulating** measurement of the accrual of energy over time
3. `mode` is a **status** message that is not a number

!!! note

	Sometimes these classifications will be hidden from you. For example SolarNetwork hides them
	when returning datum data from some SolarNetwork API methods. You might come across them in some
	SolarNode plugins that allow configuring dynamic sample properties to collect, when SolarNode
	does not implicitly know which classification to use. Some SolarNetwork APIs do return or
	require fully classified sample objects; the documentation for those services will make that
	clear.


[datum-list]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarQuery-API#datum-list
[sec-policy]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarNet-API-global-objects#security-policy
[wildcard-pats]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarNet-API-global-objects#wildcard-patterns
