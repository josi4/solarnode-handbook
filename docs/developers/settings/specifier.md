# Settings Specifier

The [`net.solarnetwork.settings.SettingSpecifier`][SettingSpecifier] API defines metadata for a
single configurable property in the Settings API. The API looks like this:

```java
public interface SettingSpecifier {

	/**
	 * A unique identifier for the type of setting specifier this represents.
	 *
	 * <p>
	 * Generally this will be a fully-qualified interface name.
	 * </p>
	 *
	 * @return the type
	 */
	String getType();

	/**
	 * Localizable text to display with the setting's content.
	 *
	 * @return the title
	 */
	String getTitle();

}
```

This interface is very simple, and extended by more specialized interfaces that form more useful
setting _types_.

!!! note

	A `SettingSpecifier` instance is often referred to simply as _a setting_.


Here is a view of the class hierarchy that builds off of this interface:

![SettingSpecifier class hierarchy diagram](../../images/developers/settings/setting-specifier-class-hierarchy.png){width=317}

!!! note

	The `SettingSpecifier` API defines metadata about a configurable property, but not methods to
	view or change that property's value. The [Settings Service](../services/settings-service.md)
	provides methods for managing setting values.

## Settings Playpen

The [Settings Playpen][playpen] plugin demonstrates most of the available setting types, and is a
great way to see how the settings can be used.

## Text Field

The [`TextFieldSettingSpecifier`][TextFieldSettingSpecifier] defines a simple string-based
configurable property and is the most common setting type. The setting defines a `key` that maps to
a setter method on its associated component class. In the SolarNode GUI a text field is rendered as
an HTML form text input, like this:

![Text field setting as an HTML form field](../../images/developers/settings/text-field-setting.png){width=416}

The `net.solarnetwork.settings.support.BasicTextFieldSettingSpecifier` class provides the standard implementation
of this API. A standard text field setting is created like this:

```java
new BasicTextFieldSettingSpecifier("myProperty", "DEFAULT_VALUE");

// or without any default value
new BasicTextFieldSettingSpecifier("myProperty", null);

```

!!! tip

	Setting values are generally treated as strings within the Settings API, however other basic
	data types such as integers and numbers can be used as well. You can also publish a "proxy"
	setting that manages a complex data type as a string, and en/decode the complex type in your
	component accessor methods.

	For example a `Map<String, String>` setting could be published as a text field setting that
	en/decodes the `Map` into a delimited string value, for example `name=Test, color=red`.

### Secure Text Field

The `BasicTextFieldSettingSpecifier` can also be used for "secure" text fields where the field's
content is obscured from view. In the SolarNode GUI a secure text field is rendered as an HTML
password form input like this:

![Secure text field setting as an HTML form field](../../images/developers/settings/secure-text-field-setting.png){width=435}

A standard secure text field setting is created by passing a third `true` argument, like this:

```java
new BasicTextFieldSettingSpecifier("myProperty", "DEFAULT_VALUE", true);

// or without any default value
new BasicTextFieldSettingSpecifier("myProperty", null, true);
```

## Text Area

The [`TextAreaSettingSpecifier`][TextAreaSettingSpecifier] defines a simple string-based
configurable property for a larger text value, loaded as an external file using the
[SettingResourceHandler](resource-handler.md) API. In the SolarNode GUI a text area is rendered
as an HTML form text area with an associated button to upload the content, like this:

![Text area setting as an HTML form field](../../images/developers/settings/text-area-setting.png){width=476}

The `net.solarnetwork.settings.support.BasicTextAreaSettingSpecifier` class provides the standard implementation
of this API. A standard text field setting is created like this:

```java
new BasicTextAreaSettingSpecifier("myProperty", "DEFAULT_VALUE");

// or without any default value
new BasicTextAreaSettingSpecifier("myProperty", null);
```

### Direct Text Area

The `BasicTextAreaSettingSpecifier` can also be used for "direct" text areas where the field's
content is not uploaded as an external file. In the SolarNode GUI a direct text area is rendered as
an HTML form text area, like this:

![Direct text area setting as an HTML form field](../../images/developers/settings/direct-text-area-setting.png){width=446}

A standard direct text area setting is created by passing a third `true` argument, like this:

```java
new BasicTextAreaSettingSpecifier("myProperty", "DEFAULT_VALUE", true);

// or without any default value
new BasicTextAreaSettingSpecifier("myProperty", null, true);
```

## Toggle

The [`ToggleSettingSpecifier`][ToggleSettingSpecifier] defines a boolean configurable property. In
the SolarNode GUI a toggle is rendered as an HTML form button, like this:

![Toggle setting as an HTML form field](../../images/developers/settings/toggle-setting.png){width=142}

The `net.solarnetwork.settings.support.BasicToggleSettingSpecifier` class provides the standard implementation
of this API. A standard toggle setting is created like this:

```java
new BasicToggleSettingSpecifier("toggle", false); // default "off"

new BasicToggleSettingSpecifier("toggle", true);  // default "on"
```

## File

TODO

[playpen]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.settings.playpen
[SettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/SettingSpecifier.html
[TextAreaSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/TextAreaSettingSpecifier.html
[TextFieldSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/TextFieldSettingSpecifier.html
[ToggleSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/ToggleSettingSpecifier.html
