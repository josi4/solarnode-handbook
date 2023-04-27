# Settings Provider

The [`net.solarnetwork.settings.SettingSpecifierProvider`][SettingSpecifierProvider] interface
defines the way a class can declare themselves as a user-configurable component. The main elements
of this API are:

```java
public interface SettingSpecifierProvider {

	/**
	 * Get a unique, application-wide setting ID.
	 *
	 * @return unique ID
	 */
	String getSettingUid();

	/**
	 * Get a non-localized display name.
	 *
	 * @return non-localized display name
	 */
	String getDisplayName();

	/**
	 * Get a list of {@link SettingSpecifier} instances.
	 *
	 * @return list of {@link SettingSpecifier}
	 */
	List<SettingSpecifier> getSettingSpecifiers();

}
```

The `getSettingUid()` method defines a unique ID for the configurable component. By convention the
class or package name of the component (or a derivative of it) is used as the ID.

The `getSettingSpecifiers()` method returns a list of all the configurable properties of the
component, as a list of [Setting Specifier](specifier.md) instances.

## Setting accessors

For each configurable property exposed as a setting, your service must provide a standard
setter method that will accept values provided by a user. For example, a
**username** setting would expect a `setUsername(String)` method on the service like this:

```java
@Override
private String username;

public List<SettingSpecifier> getSettingSpecifiers() {
	List<SettingSpecifier> results = new ArrayList<>(1);

	// expose a "username" setting with a default value of "admin"
	results.add(new BasicTextFieldSettingSpecifier("username", "admin"));

	return results;
}

// settings are updated at runtime via standard setter methods
public void setUsername(String username) {
	this.username = username;
}
```

Setting values are treated as strings within the Settings API, but the methods associated with
settings can accept any primitive or standard number type like `int` or `Integer` as well.

```java title="BigDecimal setting example"
@Override
private BigDecimal num;

public List<SettingSpecifier> getSettingSpecifiers() {
	List<SettingSpecifier> results = new ArrayList<>(1);

	results.add(new BasicTextFieldSettingSpecifier("num", null));

	return results;
}

// settings will be coerced from strings into basic types automatically
public void setNum(BigDecimal num) {
	this.num = num;
}
```


### Proxy setting accessors

Sometimes you might like to expose a simple string setting but internally treat the string as a more
complex type. For example a `Map` could be configured using a simple delimited string like `key1 =
val1, key2 = val2`. For situations like this you can publish a _proxy_ setting that manages a
complex data type as a string, and en/decode the complex type in your component accessor methods.

```java title="Delimited string to Map setting example"
@Override
private Map<String, String> map;

public List<SettingSpecifier> getSettingSpecifiers() {
	List<SettingSpecifier> results = new ArrayList<>(1);

	// expose a "mapping" proxy setting for the map field
	results.add(new BasicTextFieldSettingSpecifier("mapping", null));

	return results;
}

public void setMapping(String mapping) {
	this.map = StringUtils.commaDelimitedStringToMap(mapping);
}
```

[SettingSpecifierProvider]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/SettingSpecifierProvider.html
