# MemSQL Action


Description
-----------
Action that runs a MemSQL command.


Use Case
--------
The action can be used whenever you want to run a MemSQL command before or after a data pipeline.
For example, you may want to run a sql update command on a database before the pipeline source pulls data from tables.


Properties
----------
**Driver Name:** Name of the JDBC driver to use.

**Database Command:** Database command to execute.

**Host:** Host that MemSQL is running on.

**Port:** Port that MemSQL is running on.

**Database:** MemSQL database name.

**Username:** User identity for connecting to the specified database.

**Password:** Password to use to connect to the specified database.

**Connection Arguments:** A list of arbitrary string key/value pairs as connection arguments. These arguments
will be passed to the JDBC driver as connection arguments for JDBC drivers that may need additional configurations.

**Auto Reconnect:** Should the driver try to re-establish stale and/or dead connections.

**Use SSL:** Turns on SSL encryption. The connection will fail if SSL is not available.

**Keystore URL:** URL to the client certificate KeyStore (if not specified, use defaults). Must be accessible at the
same location on host where CDAP Master is running and all hosts on which at least one HDFS, MapReduce, or YARN daemon
role is running.

**Keystore Password:** Password for the client certificates KeyStore.

**Truststore URL:** URL to the trusted root certificate KeyStore (if not specified, use defaults). Must be accessible at
the same location on host where CDAP Master is running and all hosts on which at least one HDFS, MapReduce, or YARN
daemon role is running.

**Truststore Password:** Password for the trusted root certificates KeyStore

**Use Compression:** Use zlib compression when communicating with the server. Select this option for WAN
connections.

**Use ANSI Quotes:** Treats " as an identifier quote character and not as a string quote character.


Example
-------
Suppose you want to execute a query against a MemSQL database named "prod" that is running on "localhost" 
port 3306, then configure the plugin with:

```
Driver Name: "mariadb"
Database Command: "UPDATE table_name SET price = 20 WHERE ID = 6"
Host: "localhost"
Port: 3306
Database: "prod"
```