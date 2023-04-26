# Manifest

As SolarNode plugins are OSGi bundles, which are Java JAR files, every plugin automatically includes
a `META-INF/MANIFEST.MF` file as defined in the Java [JAR File Specification][jar]. The
`MANIFEST.MF` file is where OSGi metadata is included, turning the JAR into an OSGi bundle (plugin).

## Example

Here is an example snippet from the SolarNode [net.solarnetwork.common.jdt][nsc-jdt-manifest]
plugin:

```properties title="Example plugin MANIFEST.MF"
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Name: Java Compiler Service (JDT)
Bundle-SymbolicName: net.solarnetwork.common.jdt
Bundle-Description: Java complier using Eclipse JDT.
Bundle-Version: 3.0.0
Bundle-Vendor: SolarNetwork
Bundle-RequiredExecutionEnvironment: JavaSE-1.8
Bundle-Activator: net.solarnetwork.common.jdt.Activator
Export-Package:
 net.solarnetwork.common.jdt;version="2.0.0"
Import-Package:
 net.solarnetwork.service;version="[1.0,2.0)",
 org.eclipse.jdt.core.compiler,
 org.eclipse.jdt.internal.compiler,
 org.osgi.framework;version="[1.5,2.0)",
 org.slf4j;version="[1.7,2.0)",
 org.springframework.context;version="[5.3,6.0)",
 org.springframework.core.io;version="[5.3,6.0)",
 org.springframework.util;version="[5.3,6.0)"
```

The rest of this document will describe this structure in more detail.

## Versioning

In OSGi plugins are _always_ versioned and and Java packages _may_ be versioned.
Versions follow [Semantic Versioning][semver] rules, generally using this syntax:

```
major.minor.patch
```

In the [manifest example](#example) you can see the plugin version `3.0.0` declared in the
`Bundle-Version` attribute:

```properties
Bundle-Version: 3.0.0
```

The example also declares ([exports](#package-exports)) a `net.solarnetwork.common.jdt` package for
other plugins to import (use) as version `2.0.0`, in the `Export-Package` attribute:

```properties
Export-Package:
 net.solarnetwork.common.jdt;version="2.0.0"
```

The example also uses ([imports](#package-dependencies)) a _versioned_ package
`net.solarnetwork.service` using a _version range_ greater than or equal to `1.0` and less than
`2.0` and an _unversioned_ package `org.eclipse.jdt.core.compiler`, in the `Import-Package`
attribute:

```properties
Import-Package:
 net.solarnetwork.service;version="[1.0,2.0)",
 org.eclipse.jdt.core.compiler,
```

!!! tip

	Some plugins, and core Java system packages, do not declare package versions. You _should_
	declare package versions in your own plugins.

## Version ranges

Some OSGi version attributes allow version ranges to be declared, such as the `Import-Package`
attribute. A version range is a comma-delimited `lower,upper` specifier. Square brackets are used to
represent _inclusive_  values and round brackets represent _exclusive_ values. A value can be
omitted to reprsent an _unbounded_ value. Here are some examples:

| Range | Logic | Description |
|:------|:------|:------------|
| `[1.0,2.0)` | 1.0.0 ≤ _x_ < 2.0.0 | Greater than or equal to `1.0.0` and less than `2.0.0` |
| `(1,3)`     | 1.0.0 < _x_ < 3.0.0 | Greater than `1.0.0` and less than `3.0.0` |
| `[1.3.2,)`  | 1.3.2 ≤ _x_ | Greater than or eequal to `1.3.2` |
| `1.3.2`     | 1.3.2 ≤ _x_ | Greater than or eequal to `1.3.2` (shorthand notation) |

!!! tip "Implied unbounded range"

	An inclusive lower, unbounded upper range can be specifeid using a shorthand
	notation of just the lower bound, like `1.3.2`.

## Required attributes

Each plugin _must_ provide the following attributes:

| Attribute | Example | Description |
|:----------|:--------|:------------|
| `Bundle-ManifestVersion` | 2 | declares the OSGi bundle manifest version; always `2` |
| `Bundle-Name` | Awesome Data Source | a concise human-readable name for the plugin |
| `Bundle-SymbolicName` | com.example.awesome | a machine-readable, universally unique identifier for the plugin |
| `Bundle-Version` | 1.0.0 | the plugin [version](#versioning) |
| `Bundle-RequiredExecutionEnvironment` | JavaSE-1.8 | a required OSGi execution environment |

## Recommended attributes

Each plugin is _recommended_ to provide the following attributes:

| Attribute | Example | Description |
|:----------|:--------|:------------|
| `Bundle-Description` | An awesome data source that collects awesome data. | a longer human-readable description of the plugin |
| `Bundle-Vendor` | ACME Corp | the name of the entity or organisation that authored the plugin |

## Common attributes

Other common manifest attributes are:

| Attribute | Example | Description |
|:----------|:--------|:------------|
| `Bundle-Activator` | com.example.awesome.Activator | a fully-qualified Java class name that implements the `org.osgi.framework.BundleActivator` interface, to handle plugin lifecycle events |
| `Export-Package` | net.solarnetwork.common.jdt;version="2.0.0" | a [package export](#package-exports) list |
| `Import-Package` | net.solarnetwork.service;version="[1.0,2.0)" | a [package dependency](#package-dependencies) list |

## Package dependencies

A plugin must declare the Java packages it _directly_ uses in a `Import-Package` attribute. This
attribute accpets a comma-delimited list of _package specifications_ that take the basic form of:

```
PACKAGE;version="VERSION"
```

For example here is how the `net.solarnetwork.service` package, versioned between `1.0` and `2.0`,
would be declared:

```properties
Import-Package: net.solarnetwork.service;version="[1.0,2.0)"
```

_Direct package use_ means your plugin has code that imports a class from a given package. Classes
in an imported package may import other packages _indirectly_; you do **not** need to import those
packages as well.  For example if you have code like this:

```java
import net.solarnetwork.service.OptionalService;
```

Then you will need to import the `net.solarnetwork.service` package.

!!! note

	The SolarNode platform automatically imports core Java packages like `java.*` so you do not need
	to declare those.

	Also note that in some scenarios a package used by a class in an imported package becomes
	a direct dependency. For example when you extend a class from an imported package and that
	class imports other packages. Those other packages _may_ become direct dependencies that
	you also need to import.

### Child package dependencies

If you import a package in your plugin, any child packages that may exist are **not** imported as
well. You must import **every individual package** you need to use in your plugin.

For example to use both `net.solarnetwork.service` and `net.solarnetwork.service.support` you would
have an `Import-Package` attribute like this:

```properties
Import-Package:
 net.solarnetwork.service;version="[1.0,2.0)",
 net.solarnetwork.service.support;version="[1.1,2.0)"
```

## Package exports

A plugin can _export_ any package it provides, making the resources within that package available to
_other_ plugins to import and use. Declare exoprted packages with a `Export-Package` attribute. This
attribute takes a comma-delimited list of versioned package specifications. Note that version
_ranges_ are **not** supported: you must declare the exact version of the package you are exporting.
For example:

```properties
Export-Package: com.example.service;version="1.0.0"
```

!!! note

	Exported packages should not be confused with [services](index.md#services). Exported packages
	give other plugins access to the classes and any other resources within those packages, but do
	not provide services to the platform. You can use [Blueprint](blueprint.md#service-registration)
	to register services. Keep in mind that any service a plugin registers must exist within an
	exported package to be of any use.

[jar]: https://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html
[nsc-jdt-manifest]: https://github.com/SolarNetwork/solarnetwork-common/blob/develop/net.solarnetwork.common.jdt/META-INF/MANIFEST.MF
[semver]: https://semver.org/
