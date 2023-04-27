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
the SolarNode GUI a toggle setting is rendered as an HTML form button, like this:

![Toggle setting as an HTML form field](../../images/developers/settings/toggle-setting.png){width=142}

The `net.solarnetwork.settings.support.BasicToggleSettingSpecifier` class provides the standard implementation
of this API. A standard toggle setting is created like this:

```java
new BasicToggleSettingSpecifier("enabled", false); // default "off"

new BasicToggleSettingSpecifier("enabled", true);  // default "on"
```

## Slider

The [`SliderSettingSpecifier`][SliderSettingSpecifier] defines a number-based configuration property
with minimum and maximum values enforced, and a _step_ limit. In the SolarNode GUI a
slider is rendered as an HTML widget, like this:

![Slider setting as an HTML form widget](../../images/developers/settings/slider-setting.png){width=382}

The `net.solarnetwork.settings.support.BasicSliderSettingSpecifier` class provides the standard implementation
of this API. A standard Slider setting is created like this:

```java
// no default value, range between 0-11 in 0.5 increments
new BasicSliderSettingSpecifier("volume", null, 0.0, 11.0, 0.5);

// default value 5.0, range between 0-11 in 0.5 increments
new BasicSliderSettingSpecifier("volume", 5.0, 0.0, 11.0, 0.5);
```

## Radio Group

The [`RadioGroupSettingSpecifier`][RadioGroupSettingSpecifier] defines a configurable property that
accepts a single value from a fixed set of possible values. In the SolarNode GUI a radio group is
rendered as a set of HTML radio input form fields, like this:

![Radio Group setting as a set of HTML radio inputs](../../images/developers/settings/radio-group-setting.png){width=156}

The `net.solarnetwork.settings.support.BasicRadioGroupSettingSpecifier` class provides the standard implementation
of this API. A standard RadioGroup setting is created like this:

```java
String[] vals = new String[] {"a", "b", "c"};
String[] labels = new Strign[] {"One", "Two", "Three"};
Map<String, String> radioValues = new LinkedHashMap<>(3);
for ( int i = 0; i < vals.length; i++ ) {
	radioValues.put(vals[i], labels[i]);
}
BasicRadioGroupSettingSpecifier radio =
		new BasicRadioGroupSettingSpecifier("option", vals[0]);
radio.setValueTitles(radioValues);
```

## Multi-value

The [`MultiValueSettingSpecifier`][MultiValueSettingSpecifier] defines a configurable property that
accepts a single value from a fixed set of possible values. In the SolarNode GUI a multi-value
setting is rendered as an HTML select form field, like this:

![Multi-value setting as an HTML select form field](../../images/developers/settings/multi-value-setting.png){width=318}

The `net.solarnetwork.settings.support.BasicMultiValueSettingSpecifier` class provides the standard implementation
of this API. A standard MultiValue setting is created like this:

```java
String[] vals = new String[] {"a", "b", "c"};
String[] labels = new Strign[] {"Option 1", "Option 2", "Option 3"};
Map<String, String> radioValues = new LinkedHashMap<>(3);
for ( int i = 0; i < vals.length; i++ ) {
	radioValues.put(vals[i], labels[i]);
}
BasicMultiValueSettingSpecifier menu = new BasicMultiValueSettingSpecifier("option",
		vals[0]);
menu.setValueTitles(menuValues);
```

## File

The [`FileSettingSpecifier`][FileSettingSpecifier] defines a file-based resource property, loaded as
an external file using the [SettingResourceHandler](resource-handler.md) API. In the SolarNode GUI a
file setting is rendered as an HTML file input, like this:

![File setting as an HTML form field](../../images/developers/settings/file-setting.png){width=468}

The `net.solarnetwork.node.settings.support.BasicFileSettingSpecifier` class provides the standard implementation
of this API. A standard file setting is created like this:

```java
// a single file only, no default content
new BasicFileSettingSpecifier("document", null,
		new LinkedHashSet<>(Arrays.asList(".txt", "text/*")), false);

// multiple files allowed, no default content
new BasicFileSettingSpecifier("document-list", null,
		new LinkedHashSet<>(Arrays.asList(".txt", "text/*")), true);
```

[playpen]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.settings.playpen
[FileSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/settings/FileSettingSpecifier.html
[MultiValueSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/MultiValueSettingSpecifier.html
[RadioGroupSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/RadioGroupSettingSpecifier.html
[SettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/SettingSpecifier.html
[SliderSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/SliderSettingSpecifier.html
[TextAreaSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/TextAreaSettingSpecifier.html
[TextFieldSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/TextFieldSettingSpecifier.html
[ToggleSettingSpecifier]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/ToggleSettingSpecifier.html
