# Configuration

SolarNode components can support configurable properties grouped into _namespaces_. Each namespace
is configured in a [properties file][props-file] located at
 `${CONF_DIR}/services/${namespace}.properties`.

!!! note "Configuration location"

    The `${CONF_DIR}` location is `/etc/solarnode` in SolarNodeOS.

## Example

Imagine a component uses the configuration namespace `com.example.service` and supports a
configurable property named `max-threads` that accepts an integer value you would like to configure
as `4`. You would create a `com.example.service.properties` file like:

```properties title="${CONF_DIR}/services/com.example.service.properties"
max-threads = 4
```

[props-file]: https://en.wikipedia.org/wiki/.properties
