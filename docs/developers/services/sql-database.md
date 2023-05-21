# SQL Database

The SolarNode runtime provides a local SQL database that is used to hold application settings, data
sampled from devices, or anything really. Some data is designed to live only in this local store
(such as settings) while other data eventually gets pushed up into the SolarNet cloud. This document
describes the most common aspects of the local database.

## Database implementation

The database is provided by either the [H2][h2]
or [Apache Derby][derby] embedded SQL
database engine.

!!! note

	In SolarNodeOS the [solarnode-app-db-h2][solarnode-app-db-h2] and
	[solarnode-app-db-derby][solarnode-app-db-derby] packages provide the H2 and Derby database
	implementations. Most modern SolarNode deployments use H2.

Typically the database is configured to run entirely within RAM on devices that support it,
and the RAM copy is periodically synced to non-volatile media so if the device restarts the
persisted copy of the database can be loaded back into RAM. This pattern works well because:

 1. Non-volatile media access can be slow (e.g. flash memory)
 2. Non-volatile media can wear out over time from many writes (e.g. flash memory)
 3. Aside from settings, which change infrequently, most data stays locally only a
    short time before getting pushed into the SolarNet cloud.

## Low level access: JDBC

A standard [JDBC][jdbc] stack is available and normal SQL queries are used to access the database.
The [Hikari][hikari-cp] JDBC connection pool provides a `javax.sql.DataSource` for direct JDBC
access. The pool is configured by [factory configuration](../../users/configuration.md) files in the
`net.solarnetwork.jdbc.pool.hikari` namespace. See the
[net.solarnetwork.jdbc.pool.hikari-solarnode.cfg][h2-factory-conf] as an example.

To make use of the `DataSource` from a plugin using [OSGi Blueprint](../osgi/blueprint.md) you can
declare a reference like this:

```xml
<reference id="dataSource" interface="javax.sql.DataSource" filter="(db=node)" />
```

The [net.solarnetwork.node.dao.jdbc][node-dao-jdbc] bundle publishes some other JDBC services for
plugins to use, such as:

 * A Spring `org.springframework.jdbc.core.JdbcOperations` for slightly higher-level JDBC access
 * A Spring `org.springframework.transaction.PlatformTransactionManager` for JDBC transaction
   support

To make use of these from a plugin using [OSGi Blueprint](../osgi/blueprint.md) you can
declare references to these APIs like this:

```xml
<reference id="jdbcOps" interface="org.springframework.jdbc.core.JdbcOperations"
		filter="(db=node)" />

<reference id="txManager" interface="org.springframework.transaction.PlatformTransactionManager"
		filter="(db=node)" />
```

## High level access: Data Access Object (DAO)

The SolarNode runtime also provides some Data Access Object (DAO) services that
make storing some typical data easier:

 * A `net.solarnetwork.node.dao.SettingDao` for access to the [Settings Database](settings-db.md)
 * A `net.solarnetwork.node.dao.DatumDao` for access to the [Datum Database](datum-db.md)

To make use of these from a plugin using [OSGi Blueprint](../osgi/blueprint.md) you can
declare references to these APIs like this:

```xml
<reference id="settingDao" interface="net.solarnetwork.node.dao.SettingDao"/>

<reference id="datumDao" interface="net.solarnetwork.node.dao.DatumDao"/>
```

[derby]: https://db.apache.org/derby/
[jdbc]: https://en.wikipedia.org/wiki/Java_Database_Connectivity
[h2]: https://h2database.com/
[h2-factory-conf]: https://github.com/SolarNetwork/solarnode-os-packages/tree/develop/solarnode-app-db-h2/debian/etc/solarnode/services/net.solarnetwork.jdbc.pool.hikari-solarnode.cfg
[hikari-cp]: https://github.com/brettwooldridge/HikariCP
[node-dao-jdbc]: https://github.com/SolarNetwork/solarnetwork-node/blob/master/net.solarnetwork.node.dao.jdbc
[solarnode-app-db-derby]: https://github.com/SolarNetwork/solarnode-os-packages/tree/develop/solarnode-app-db-derby/debian
[solarnode-app-db-h2]: https://github.com/SolarNetwork/solarnode-os-packages/tree/develop/solarnode-app-db-h2/debian
