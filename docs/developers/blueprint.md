# Blueprint

SolarNode supports the OSGi Blueprint Container Specification so plugins can declare their service
dependencies and publish their services by way of an XML file deployed with the plugin. If you are
familiar with the [Spring Framework's XML configuration][spring-xml-config], you will find Blueprint
very similar. SolarNode uses the [Eclipse Gemini][eclipse-gemini-blueprint] implementation of the
Blueprint specification, which is directly derived from Spring Framework.

!!! note
	This guide will not document the full Blueprint XML syntax. Rather, it will attempt to showcase the
	most common parts used in SolarNode. Refer to the [Blueprint Container
	Specification][osgi-blueprint] for full details of the specification.

## Blueprint XML Resources

Blueprint XML documents are added to a plugin's `OSGI-INF/blueprint` classpath location. A plugin
can provide any number of Blueprint XML documents there, but often a single file is sufficient and
a common convention in SolarNode is to name it `module.xml`.

## XML Syntax


A minimal Blueprint XML file is structured like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
		http://www.osgi.org/xmlns/blueprint/v1.0.0
		http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd">

	<!-- Plugin components configured here -->

</blueprint>
```

### Service References

To make use of services published by SolarNode plugins, you declare a _reference_ to that service so
you may refer to it in your own component. For example, imagine you wanted to use the [Placeholder
Service](services/placeholder-service.md) in your component. You would obtain a reference to that
like this:

```xml
<reference id="placeholderService"
	interface="net.solarnetwork.node.service.PlaceholderService"/>
```

The `id` attribute allows you to refer to this service elsewhere in your Blueprint XML, while
`interface` declares the fully-qualified Java interface of the service you want to use.

### Components

Components in Blueprint are Java classes you would like instantiated when your plugin starts. They are
declared using a `<bean>` element in Blueprint XML. You can assign each component a unique identifier using
an `id` attribute, and then you can refer to that component in other components.

Imagine an example component class `com.example.MyComponent`:

```java
package com.example;

import net.solarnetwork.node.service.PlaceholderService;

public class MyComponent {

	private final PlaceholderService placeholderService;
	private int minimum;

	public MyComponent(PlaceholderService placeholderService) {
		super();
		this.placeholderService = placeholderService;
	}

	public String go() {
		return PlaceholderService.resolvePlaceholders(placeholderService,
				"{building}/temp", null);
	}

	public int getMinimum() {
		return minimum;
	}

	public void setMinimum(int minimum) {
		this.minimum = minimum;
	}
}
```

Here is how that component could be written in Blueprint:

```xml
<reference id="placeholderService" interface="net.solarnetwork.node.service.PlaceholderService"/>

<bean id="myComponent" class="com.example.MyComponent">
	<argument ref="placeholderService">
	<property name="minimum" value="10"/>
</bean>
```

#### Constructor Arguments

If your component requires any constructor arguments, they can be specified with nested `<argument>`
elements in Blueprint. The `<argument>` value can be specified as a reference to another component
using a `ref` attribute whose value is the `id` of that component, or as a literal value using a
`value` attribute.

For example:

```xml
<bean id="myComponent" class="com.example.MyComponent">
	<argument ref="placeholderService">
	<argument value="10">
```

#### Property Accessors

You can configure mutable class properties on a component with nested `<property name="">` elements in Blueprint.
The `<property>` value can be specified as a reference to another component
using a `ref` attribute whose value is the `id` of that component, or as a literal value using a
`value` attribute.

For example:


```xml
<bean id="myComponent" class="com.example.MyComponent">
	<property name="placeholderService" ref="placeholderService">
	<argument name="minimum" value="10">
```

#### Start/Stop Hooks

Blueprint can invoke a method on your component when it has finished instantiating and configuring
the object (when the plugin starts), and another when it destroys the instance (when the plugin is
stopped). You simply provide the name of the method you would like Blueprint to call in the
`init-method` and `destroy-method` attributes of the `<bean>` element. For example:

```xml
<bean id="myComponent" class="com.example.MyComponent"
	init-method="startup"
	destroy-method="shutdown">
```

### Published Services

You can make any component available to other plugins by _publishing_ the component with a
`<service>` element that declares what interface(s) your component provides. Once published, other
plugins can make use of your component, for example by declaring a `<referenece>` to your component
class in _their_ Blueprint XML.

!!! note
	You can only publish Java _interfaces_ as services, not _classes_.

For example, imagine a `com.example.Startable` interface like this:

```java
package com.example;
public interface Startable {
	/**
	 * Start!
	 * @return the result
	 */
	String go();
}
```

We could implement that interface in the `MyComponent` class, like this:

```java
package com.example;

public class MyComponent implements Startable {

	@Override
	public String go() {
		return "Gone!";
	}
}
```

We can publish `MyComponent` as a `Startable` service like this in Blueprint:

```xml
<service ref="myComponent" interface="com.example.Startable"/>
```

#### Multiple Service Interfaces

You can advertise any number of service interfaces that your component supports, by nesting an `<interfaces>`
element within the `<service>` element, in place of the `interface` attribute. For example:

```xml
<service ref="myComponent">
	<interfaces>
		<value>com.example.Startable</value>
		<value>com.example.Stopable</value>
	</interfaces>
</service>
```

[eclipse-gemini-blueprint]: https://eclipse.org/gemini/blueprint/
[osgi-blueprint]: https://docs.osgi.org/specification/osgi.cmpn/7.0.0/service.blueprint.html
[spring-xml-config]: https://docs.spring.io/spring-framework/docs/5.3.0/reference/html/core.html#beans-factory-metadata
