# Settings Provider

The [`net.solarnetwork.settings.SettingSpecifierProvider`][SettingSpecifierProvider] interface defines the way a class can
declare themselves as a configurable component. The main elements of this API are:

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
component, as a list of `SettingSpecifier` instances.


[SettingSpecifierProvider]: https://javadoc.io/doc/net.solarnetwork.common/net.solarnetwork.common/latest/net/solarnetwork/settings/SettingSpecifierProvider.html
