# Operational Modes

SolarNode supports a concept called **operational modes**. Modes are simple names like `quiet` and
`hyper` that can be either **active** or **inactive**. Any number of modes can be active at a given
time. In theory both `quiet` and `hyper` could be active simultaneously. Modes can be named anything
you like.

Modes can be used by SolarNode components to alter their behavior dynamically. For example a data
source component might stop collecting data from a set of data sources if the `quiet` mode is
active, or start collecting data at an increased frequency if `hyper` is active. Some components
might require specific names, which are described in their documentation. Components that allow
configuring a required operational mode setting can also invert the requirement by adding a `!`
prefix to the mode name, for example `!hyper` can be thought of as _"when `hyper` is **not**
active"_. You can also specify exactly `!` to match only when _no_ mode is active.

[Datum Filters](./datum-filters/index.md) also make use of operational modes, to toggle filters
on and off dynamically.

## Automatic expiration

Operational modes can be activated with an associated **expiration date**. The mode will remain
active until the expiration date, at which time it will be automatically deactivated. A mode can
always be manually deactivated before its associated expiration date.

## Operational Modes management API

The [SolarUser Instruction API][add-instr] can be used to toggle operational modes on and off. The
[`EnableOperationalModes`][enable-instr] instruction activates modes and
[`DisableOperationalModes`][disable-instr] deactivates them.

[add-instr]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-API#queue-instruction
[disable-instr]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-API-enumerated-types#disableoperationalmodes
[enable-instr]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarUser-API-enumerated-types#enableoperationalmodes
