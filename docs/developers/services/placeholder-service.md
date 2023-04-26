# Placeholder Service

The [Placeholder Service][PlaceholderService] API provides components a way to resolve variables in
strings, known as _placeholders_, whose values are managed outside the component itself. For example
a datum data source plugin could use the Placeholder Service to support resolving placeholders in a
configurable Source ID property.

SolarNode provides a [Placeholder Service implementation][SettingsPlaceholderService] that resolves
both dynamic placeholders from the [Settings Database](settings-db.md) (using the setting namespace
`placeholder`), and static placeholders from a configurable file or directory location.

## Use

Call the `resolvePlaceholders(s, parameters)` method to resolve all placeholders on the String `s`.
The `parameters` argument can be used to provide additional placeholder values, or you can pass just
pass `null` to rely solely on the placeholders available in the service already.

### Example

Here is an imaginary class that is constructed with an optional `PlaceholderService`, and then when
the `go()` method is called uses that to resolve placeholders in the string `{building}/temp` and
return the result:

```java
package com.example;

import net.solarnetwork.node.service.PlaceholderService;
import net.solarnetwork.service.OptionalService;

public class MyComponent {

	private final OptionalService<PlaceholderService> placeholderService;

	public MyComponent(OptionalService<PlaceholderService> placeholderService) {
		super();
		this.placeholderService = placeholderService;
	}

	public String go() {
		return PlaceholderService.resolvePlaceholders(placeholderService,
				"{building}/temp", null);
	}
}
```

## Blueprint

To use the Placeholder Service in your component, add either an [Optional Service][OptionalService]
or explicit reference to your plugin's [Blueprint](../osgi/blueprint.md) XML file like this
(depending on what your plugin requires):

=== "Optional Service"

	```xml
	<bean id="placeholderService" class="net.solarnetwork.common.osgi.service.DynamicServiceTracker">
		<argument ref="bundleContext"/>
		<property name="serviceClassName" value="net.solarnetwork.node.service.PlaceholderService"/>
		<property name="sticky" value="true"/>
	</bean>
	```

=== "Explicit Reference"

	```xml
	<reference id="placeholderService" interface="net.solarnetwork.node.service.PlaceholderService"/>
	```

Then inject that service into your component's `<bean>`, for example:

```xml
<bean id="myComponent" class="com.example.MyComponent">
	<argument ref="placeholderService">
</bean>
```

## Configuration

The Placeholder Service supports the following [configuration
properties](../../users/configuration.md) in the `net.solarnetwork.node.core` namespace:

<div markdown="1" class="props-explicit-col-widths">

| Property | Default | Description |
|:---------|:--------|:------------|
| `placeholders.dir` | ${CONF_DIR}/placeholders.d | Path to a single propertites file or to a directory of properties files to load as static placeholder parameter values when SolarNode starts up. |

</div>


[OptionalService]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/service/OptionalService.html
[PlaceholderService]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/service/PlaceholderService.html
[SettingsPlaceholderService]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/service/support/SettingsPlaceholderService.html
