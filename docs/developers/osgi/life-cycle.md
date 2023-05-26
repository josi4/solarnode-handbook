# Life cycle

Plugins in SolarNode can be added to and removed from the platform at any time without restarting
the SolarNode process, because of the [Life Cycle][osgi-lifecycle] process OSGi manages. The life
cycle of a plugin consists of a set of _states_ and OSGi will transition a plugin's state over the
course of the plugin's life.

The available plugin states are:

| State | Description |
|:------|:------------|
| `INSTALLED`   | The plugin has been successfully added to the OSGi framework. |
| `RESOLVED`    | All package dependencies that the bundle needs are available. This state indicates that the plugin is either ready to be started or has stopped. |
| `STARTING`    | The plugin is being started by the OSGi framework, but it has not finished starting yet. |
| `ACTIVE `     | The plugin has been successfully started and is running. |
| `STOPPING`    | The plugin is being stopped by the OSGi framework, but it has not finished stopping yet. |
| `UNINSTALLED` | The plugin has been removed by the OSGi framework. It cannot change to another state. |

The possible changes in state can be visualized in the following state-change diagram:

<figure markdown>
  ![OSGi Life Cycle state change diagram](../../../images/developers/osgi/OSGi_Bundle_Life-Cycle.svg)
  <figcaption markdown>Faisal.akeel, Public domain, via [Wikimedia Common](https://commons.wikimedia.org/wiki/File:OSGi_Bundle_Life-Cycle.svg)</figcaption>
</figure>

## Activator

A plugin can opt in to receiving callbacks for the start/stop state transitions by providing an
`org.osgi.framework.BundleActivator` implementation and declaring that class in the
[`Bundle-Activator` manifest attribute](manifest.md#common-attributes). This can be useful when a
plugin needs to initialize some resources when the plugin is started, and then release those
resources when the plugin is stopped.

=== "BundleActivator API"

	```java
	public interface BundleActivator {
		/**
		 * Called when this bundle is started so the Framework can perform the
		 * bundle-specific activities necessary to start this bundle.
		 *
		 * @param context The execution context of the bundle being started.
		 */
		public void start(BundleContext context) throws Exception;

		/**
		 * Called when this bundle is stopped so the Framework can perform the
		 * bundle-specific activities necessary to stop the bundle.
		 *
		 * @param context The execution context of the bundle being stopped.
		 */
		public void stop(BundleContext context) throws Exception;
	}
	```

=== "BundleActivator implementation example"

	```java
	package com.example.activator;
	import org.osgi.framework.BundleActivator;
	import org.osgi.framework.BundleContext;
	public class Activator implements BundleActivator {

		@Override
		public void start(BundleContext bundleContext) throws Exception {
			// initialize resources here
		}

		@Override
		public void stop(BundleContext bundleContext) throws Exception {
			// clean up resources here
		}
	}
	```

=== "Manifest declaration example"

	```properties
	Manifest-Version: 1.0
	Bundle-ManifestVersion: 2
	Bundle-Name: Example Activator
	Bundle-SymbolicName: com.example.activator
	Bundle-Version: 1.0.0
	Bundle-Activator: com.example.activator.Activator
	Import-Package: org.osgi.framework;version="[1.3,2.0)"
	```

!!! tip

	Often making use of the [component life cycle hooks](blueprint.md#startstop-hooks) available in
	Blueprint are sufficient and no `BundleActivator` is necessary.

[osgi-lifecycle]: https://en.wikipedia.org/wiki/OSGi#Life-cycle
