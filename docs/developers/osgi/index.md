# Plugins

The SolarNode platform has been designed to be highly modular and dynamic, by using a plugin-based
architecture. The plugin system SolarNode uses is based on the [OSGi][osgi] specification, where
plugins are implemented as [OSGi bundles][bundles]. SolarNode can be thought of as a collection of
OSGi bundles that, when combined and deployed together in an OSGi framework like [Eclipse
Equinox][eclipse-equinox], form the complete SolarNode platform.

To summarize: _everything_ in SolarNode is a plugin!

!!! note "OSGi bundles and Eclipse plug-ins"

	Each OSGi bundle in [SolarNode][solarnode-repo] comes configured as an [Eclipse
	IDE][eclipse-ide] (or simply Eclipse) plug-in project. Eclipse refers to OSGi bundles as
	"plug-ins" and its OSGi development tools are collectively known as the _Plug-in Development
	Environment_, or [PDE][pde] for short. We use the terms _bundle_ and _plug-in_ and _plugin_
	somewhat interchangably in the SolarNode project. Although Eclipse is not actually required for
	SolarNode development, it is very convenient.

Practically speaking a plugin, which is an OSGi bundle, is simply a Java JAR file that includes
the Java code implementing your plugin and some OSGi metadata in its [Manifest](manifest.md).

## Services

Central to the plugin architecture SolarNode uses is the concept of a _service_. In SolarNode a
service is defined by a Java interface. A plugin can _advertise_ a service to the SolarNode runtime.
Plugins can _lookup_ a service in the SolarNode runtime and then invoke the methods defined on it.

The advertising/lookup framework SolarNode uses is provided by OSGi. OSGi provides several ways to
manage services. In SolarNode the most common is to use [Blueprint XML](blueprint.md) documents to
both publish services (advertise) and acquire references to services (lookup).

[eclipse-equinox]: https://en.wikipedia.org/wiki/Equinox_(OSGi)
[eclipse-ide]: https://www.eclipse.org/ide/
[bundles]: https://en.wikipedia.org/wiki/OSGi#Bundles
[osgi]: https://en.wikipedia.org/wiki/OSGi
[pde]: https://www.eclipse.org/pde/
[solarnode-repo]: https://github.com/SolarNetwork/solarnetwork-node/
