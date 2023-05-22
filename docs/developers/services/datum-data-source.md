# Datum Data Source

The [`DatumDataSource`][DatumDataSource] API defines the primary way for plugins to generate _datum_ instances
from devices or services integrated with SolarNode, through a request-based API. The [`MultiDatumDataSource`][MultiDatumDataSource] API
is closely related, and allows a plugin to generate multiple datum when requested.

=== "DatumDataSource"

	```java
	package net.solarnetwork.node.service;

	import net.solarnetwork.node.domain.datum.NodeDatum;
	import net.solarnetwork.service.Identifiable;

	/**
	 * API for collecting {@link NodeDatum} objects from some device.
	 */
	public interface DatumDataSource extends Identifiable, DeviceInfoProvider {

		/**
		 * Get the class supported by this DataSource.
		 *
		 * @return class
		 */
		Class<? extends NodeDatum> getDatumType();

		/**
		 * Read the current value from the data source, returning as an unpersisted
		 * {@link NodeDatum} object.
		 *
		 * @return Datum
		 */
		NodeDatum readCurrentDatum();

	}
	```

=== "MultiDatumDataSource"

	```java
	package net.solarnetwork.node.service;

	import java.util.Collection;
	import net.solarnetwork.node.domain.datum.NodeDatum;
	import net.solarnetwork.service.Identifiable;

	/**
	 * API for collecting multiple {@link NodeDatum} objects from some device.
	 */
	public interface MultiDatumDataSource extends Identifiable, DeviceInfoProvider {

		/**
		 * Get the class supported by this DataSource.
		 *
		 * @return class
		 */
		Class<? extends NodeDatum> getMultiDatumType();

		/**
		 * Read multiple values from the data source, returning as a collection of
		 * unpersisted {@link NodeDatum} objects.
		 *
		 * @return Datum
		 */
		Collection<NodeDatum> readMultipleDatum();

	}
	```

The [Datum Data Source Poll Job](datum-data-source-poll-job.md) provides a way to let users schedule
the polling for datum from a data source.


[DatumDataSource]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/service/DatumDataSource.html
[MultiDatumDataSource]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/service/MultiDatumDataSource.html
