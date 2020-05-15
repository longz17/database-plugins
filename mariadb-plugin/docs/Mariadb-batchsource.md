# MariaDB Batch Source


Description
-----------
Reads from a MariaDB instance using a configurable SQL query.
Outputs one record for each row returned by the query.


Use Case
--------
The source is used whenever you need to read from a MariaDB instance. For example, you may want
to create daily snapshots of a database table by using this source and writing to
a TimePartitionedFileSet.


Properties
----------
**Reference Name:** Name used to uniquely identify this source for lineage, annotating metadata, etc.

**Driver Name:** Name of the JDBC driver to use.

**Host:** Host that MariaDB is running on.

**Port:** Port that MariaDB is running on.

**Database:** MariaDB database name.

**Import Query:** The SELECT query to use to import data from the specified table.
You can specify an arbitrary number of columns to import, or import all columns using \*. The Query should
contain the '$CONDITIONS' string. For example, 'SELECT * FROM table WHERE $CONDITIONS'.
The '$CONDITIONS' string will be replaced by 'splitBy' field limits specified by the bounding query.
The '$CONDITIONS' string is not required if numSplits is set to one.

**Bounding Query:** Bounding Query should return the min and max of the values of the 'splitBy' field.
For example, 'SELECT MIN(id),MAX(id) FROM table'. Not required if numSplits is set to one.

**Split-By Field Name:** Field Name which will be used to generate splits. Not required if numSplits is set to one.

**Number of Splits to Generate:** Number of splits to generate.

**Username:** User identity for connecting to the specified database.

**Password:** Password to use to connect to the specified database.

**Connection Arguments:** A list of arbitrary string key/value pairs as connection arguments. These arguments
will be passed to the JDBC driver as connection arguments for JDBC drivers that may need additional configurations.

**Auto Reconnect:** Should the driver try to re-establish stale and/or dead connections.

**Schema:** The schema of records output by the source. This will be used in place of whatever schema comes
back from the query. However, it must match the schema that comes back from the query,
except it can mark fields as nullable and can contain a subset of the fields.

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

**SQL_MODE:** Override the default SQL_MODE session variable used by the server.


Data Types Mapping
----------

    +--------------------------------+-----------------------+------------------------------------+
    |        MariaDB Data Type       | CDAP Schema Data Type |               Comment              |
    +--------------------------------+-----------------------+------------------------------------+
    | TINYINT                        | int                   |                                    |
    | BOOLEAN, BOOL                  | boolean               |                                    |
    | SMALLINT                       | int                   |                                    |
    | MEDIUMINT                      | int                   |                                    |
    | INT, INTEGER                   | int                   |                                    |
    | BIGINT                         | long                  |                                    |
    | DECIMAL, DEC, NUMERIC, FIXED   | decimal               |                                    |
    | FLOAT                          | float                 |                                    |
    | DOUBLE, DOUBLE PRECISION, REAL | decimal               |                                    |
    | BIT                            | boolean               |                                    |
    | CHAR                           | string                |                                    |
    | VARCHAR                        | string                |                                    |
    | BINARY                         | bytes                 |                                    |
    | CHAR BYTE                      | bytes                 |                                    |
    | VARBINARY                      | bytes                 |                                    |
    | TINYBLOB                       | bytes                 |                                    |
    | BLOB                           | bytes                 |                                    |
    | MEDIUMBLOB                     | bytes                 |                                    |
    | LONGBLOB                       | bytes                 |                                    |
    | TINYTEXT                       | string                |                                    |
    | TEXT                           | string                |                                    |
    | MEDIUMTEXT                     | string                |                                    |
    | LONGTEXT                       | string                |                                    |
    | JSON                           | string                | In MariaDB it is alias to LONGTEXT |
    | ENUM                           | string                | Mapping to String by default       |
    | SET                            | string                |                                    |
    | DATE                           | date                  |                                    |
    | TIME                           | time_micros           |                                    |
    | DATETIME                       | timestamp_micros      |                                    |
    | TIMESTAMP                      | timestamp_micros      |                                    |
    | YEAR                           | date                  |                                    |
    +--------------------------------+-----------------------+------------------------------------+


Example
------
Suppose you want to read data from MariaDB database named "prod" that is running on "localhost" port 3306,
as "root" user with "root" password,  then configure plugin with: 


```
Reference Name: "src1"
Driver Name: "mariadb"
Host: "localhost"
Port: 3306
Database: "prod"
Import Query: "select id, name, email, phone from users;"
Number of Splits to Generate: 1
Username: "root"
Password: "root"
```  

For example, if the 'id' column is a primary key of type int and the other columns are
non-nullable varchars, output records will have this schema:

    +----------------+---------------------+
    | Field Name     | Type                |
    +----------------+---------------------+
    | id             | int                 |
    | name           | string              |
    | email          | string              |
    | phone          | string              |
    +----------------+---------------------+