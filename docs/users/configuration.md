# Configuration

Some SolarNode components can be configured from properties files. This type of configuration is
meant to be changed just once, when a SolarNode is first deployed, to alter some default
configuration value. The [properties files][props-file] are read from the `/etc/solarnode/services`
directory and named like `NAMESPACE.cfg` , where `NAMESPACE` represents a _configuration namespace_.

!!! note "Configuration location"

    The `/etc/solarnode/services` location is the default location in SolarNodeOS. It might be
	another location in other SolarNode deployments.

## Example

Imagine a component uses the configuration namespace `com.example.service` and supports a
configurable property named `max-threads` that accepts an integer value you would like to configure
as `4`. You would create a `com.example.service.properties` file like:

```properties title="/etc/solarnode/services/com.example.service.cfg"
max-threads = 4
```

[props-file]: https://en.wikipedia.org/wiki/.properties
