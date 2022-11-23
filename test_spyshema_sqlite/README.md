How to document SQLite database with SchemaSpy?
==================================================

I want to create documentation of SQLite database with [ShemaSpy](https://schemaspy.org/). But I cannot figure out, how to use it to get the desired output.
Could you help me to do this?

The way I tried is sescribed below.

Setup
------

I created a directory "/mnt/d/test" on Windows Subsystem for Linux (WSL) with the following structure:

```
test
├── html/
├── podcast-reviews-database.sqlite
├── schemaspy-6.1.0.jar
├── schemaspy.properties
└── sqlite-jdbc-3.40.0.0.jar
```

- `podcast-reviews-database.sqlite` was downloaded from [Kaggle](https://www.kaggle.com/thoughtvector/podcastreviews) and renamed.
- `schemaspy-6.1.0.jar` downloaded from [github.com/schemaspy](https://github.com/schemaspy/schemaspy/releases).
- `sqlite-jdbc-3.40.0.0.jar` downloaded from [github.com/xerial/sqlite-jdbc](https://github.com/xerial/sqlite-jdbc/releases).

<details><summary> Contents of <code>schemaspy.properties</code> </summary>

(More about this file on https://schemaspy.readthedocs.io/en/latest/started.html#configuration)

```
# type of database. Run with -dbhelp for details
schemaspy.t=sqlite

# optional path to alternative jdbc drivers.
schemaspy.dp=sqlite-jdbc-3.40.0.0.jar

# output dir to save generated files
schemaspy.o=html/

# db scheme for which generate diagrams
schemaspy.s=dbo
```

---

</details>

Java is installed.

The files can be found in this repo.

Subfolder `html` should also be created.

The code that caused the issue
------------------------------

 I ran the code:
```sh
java -jar schemaspy-6.1.0.jar -t sqlite -db podcast-reviews-database.sqlite -sso -debug
```

And got an error (no desired output was created):

```
WARN  - Connection Failure
org.schemaspy.input.dbms.exceptions.ConnectionFailure: 
Failed to connect to database URL [jdbc:sqlite:/podcast-reviews-database.sqlite]
Failed to create any of 'SQLite.JDBCDriver' driver from 
driverPath 'sqlite-jdbc-3.40.0.0.jar' with sibling jars no.
```



Details:
------------

Execution of:
```sh
java -jar schemaspy-6.1.0.jar -t sqlite -db podcast-reviews-database.sqlite -sso -debug
```

Resulted in:

<details>

```
  ____       _                          ____
 / ___|  ___| |__   ___ _ __ ___   __ _/ ___| _ __  _   _ 
 \___ \ / __| '_ \ / _ \ '_ ` _ \ / _` \___ \| '_ \| | | |
  ___) | (__| | | |  __/ | | | | | (_| |___) | |_) | |_| |
 |____/ \___|_| |_|\___|_| |_| |_|\__,_|____/| .__/ \__, |
                                             |_|    |___/ 

                                              6.1.0

SchemaSpy generates an HTML representation of a database schema's relationships.
SchemaSpy comes with ABSOLUTELY NO WARRANTY.
SchemaSpy is free software and can be redistributed under the conditions of LGPL version 3 or later.
http://www.gnu.org/licenses/

INFO  - Starting Main v6.1.0 on ASUS-VG with PID 5734 (/mnt/d/test/schemaspy-6.1.0.jar started by gegznav in /mnt/d/test)
INFO  - The following profiles are active: default
INFO  - Found configuration file: schemaspy.properties
INFO  - Started Main in 3.093 seconds (JVM running for 4.159)
DEBUG - Debug enabled
INFO  - Loaded configuration from schemaspy.properties
INFO  - Starting schema analysis
DEBUG - Resolving dbType: sqlite ->
        schemaspy-6.1.0.jar!/BOOT-INF/classes!/org/schemaspy/types/sqlite.properties
```
</details>
```
DEBUG - DbSpecificOption name: 'db' value: 'podcast-reviews-database.sqlite' description: 'path to database or :memory:'
DEBUG - Unable to find driverClass 'SQLite.JDBCDriver'
WARN  - Connection Failure
org.schemaspy.input.dbms.exceptions.ConnectionFailure: Failed to connect to database URL [jdbc:sqlite:/podcast-reviews-database.sqlite] Failed to create any of 'SQLite.JDBCDriver' driver from driverPath 'sqlite-jdbc-3.40.0.0.jar' with sibling jars no.
```
<details>
```
Resulting in classpath:
        file:/mnt/d/test/sqlite-jdbc-3.40.0.0.jar

        at org.schemaspy.input.dbms.DbDriverLoader.getConnection(DbDriverLoader.java:101)
        at org.schemaspy.input.dbms.DbDriverLoader.getConnection(DbDriverLoader.java:75)
        at org.schemaspy.input.dbms.service.SqlService.connect(SqlService.java:70)
        at org.schemaspy.SchemaAnalyzer.analyze(SchemaAnalyzer.java:220)
        at org.schemaspy.SchemaAnalyzer.analyze(SchemaAnalyzer.java:123)
        at org.schemaspy.cli.SchemaSpyRunner.runAnalyzer(SchemaSpyRunner.java:98)
        at org.schemaspy.cli.SchemaSpyRunner.run(SchemaSpyRunner.java:87)
        at org.schemaspy.Main.main(Main.java:55)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.base/java.lang.reflect.Method.invoke(Method.java:566)
        at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:48)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:87)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:50)
        at org.springframework.boot.loader.JarLauncher.main(JarLauncher.java:51)
Caused by: org.schemaspy.input.dbms.exceptions.ConnectionFailure: Failed to create any of 'SQLite.JDBCDriver' driver from driverPath 'sqlite-jdbc-3.40.0.0.jar' with sibling jars no.
Resulting in classpath:
        file:/mnt/d/test/sqlite-jdbc-3.40.0.0.jar

        at org.schemaspy.input.dbms.DbDriverLoader.getDriver(DbDriverLoader.java:147)
        at org.schemaspy.input.dbms.DbDriverLoader.getConnection(DbDriverLoader.java:93)
```
</details>


Execution of:

```sh
java -jar schemaspy-6.1.0.jar -t sqlite-xerial -db podcast-reviews-database.sqlite -sso -debug
```

(Here it it was changed to `-t sqlite-xerial`)

Resulted in:

<details>
```  ____       _                          ____
 / ___|  ___| |__   ___ _ __ ___   __ _/ ___| _ __  _   _
 \___ \ / __| '_ \ / _ \ '_ ` _ \ / _` \___ \| '_ \| | | |
  ___) | (__| | | |  __/ | | | | | (_| |___) | |_) | |_| |
 |____/ \___|_| |_|\___|_| |_| |_|\__,_|____/| .__/ \__, |
                                             |_|    |___/

                                              6.1.0

SchemaSpy generates an HTML representation of a database schema's relationships.
SchemaSpy comes with ABSOLUTELY NO WARRANTY.
SchemaSpy is free software and can be redistributed under the conditions of LGPL version 3 or later.
http://www.gnu.org/licenses/

INFO  - Starting Main v6.1.0 on ASUS-VG with PID 113 (/mnt/d/Dokumentai/Data Science/TC/Materials and Projects/ds-help/test_spyshema_sqlite/schemaspy-6.1.0.jar started by gegznav in /mnt/d/Dokumentai/Data Science/TC/Materials and Projects/ds-help/test_spyshema_sqlite)
INFO  - The following profiles are active: default
INFO  - Found configuration file: schemaspy.properties
INFO  - Started Main in 3.031 seconds (JVM running for 4.097)
DEBUG - Debug enabled
INFO  - Loaded configuration from schemaspy.properties
INFO  - Starting schema analysis
DEBUG - Resolving dbType: sqlite-xerial ->
        /mnt/d/Dokumentai/Data%20Science/TC/Materials%20and%20Projects/ds-help/test_spyshema_sqlite/schemaspy-6.1.0.jar!/BOOT-INF/classes!/org/schemaspy/types/sqlite-xerial.properties
DEBUG - DbSpecificOption name: 'db' value: 'podcast-reviews-database.sqlite' description: 'path to database or :memory:'
DEBUG - supportsSchemasInTableDefinitions: false
DEBUG - supportsCatalogsInTableDefinitions: false
DEBUG - Catalog not provided, queried jdbc driver and got 'null'
DEBUG - Command line parameters: [-t, sqlite-xerial, -db, podcast-reviews-database.sqlite, -sso, -debug]
```
</details>

```
ERROR - Bad config
org.schemaspy.model.InvalidConfigurationException: Catalog (-cat) was not provided and unable to deduce catalog, wildcard catalog can be used -cat %
        at org.schemaspy.input.dbms.CatalogResolver.resolveCatalog(CatalogResolver.java:51)
        at org.schemaspy.SchemaAnalyzer.analyze(SchemaAnalyzer.java:227)
        at org.schemaspy.SchemaAnalyzer.analyze(SchemaAnalyzer.java:123)
        at org.schemaspy.cli.SchemaSpyRunner.runAnalyzer(SchemaSpyRunner.java:98)
        at org.schemaspy.cli.SchemaSpyRunner.run(SchemaSpyRunner.java:87)
        at org.schemaspy.Main.main(Main.java:55)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.base/java.lang.reflect.Method.invoke(Method.java:566)
        at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:48)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:87)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:50)
        at org.springframework.boot.loader.JarLauncher.main(JarLauncher.java:51)
```


Next, 

```sh
aspy-6.1.0.jar -t sqlite-xerial -db podcast-reviews-database.sqlite -sso -cat %
```


resulted in:

<details>
```
  ____       _                          ____
 / ___|  ___| |__   ___ _ __ ___   __ _/ ___| _ __  _   _
 \___ \ / __| '_ \ / _ \ '_ ` _ \ / _` \___ \| '_ \| | | |
  ___) | (__| | | |  __/ | | | | | (_| |___) | |_) | |_| |
 |____/ \___|_| |_|\___|_| |_| |_|\__,_|____/| .__/ \__, |
                                             |_|    |___/

                                              6.1.0

SchemaSpy generates an HTML representation of a database schema's relationships.
SchemaSpy comes with ABSOLUTELY NO WARRANTY.
SchemaSpy is free software and can be redistributed under the conditions of LGPL version 3 or later.
http://www.gnu.org/licenses/

INFO  - Starting Main v6.1.0 on ASUS-VG with PID 336 (/mnt/d/Dokumentai/Data Science/TC/Materials and Projects/ds-help/test_spyshema_sqlite/schemaspy-6.1.0.jar started by gegznav in /mnt/d/Dokumentai/Data Science/TC/Materials and Projects/ds-help/test_spyshema_sqlite)
INFO  - The following profiles are active: default
INFO  - Found configuration file: schemaspy.properties
INFO  - Started Main in 2.928 seconds (JVM running for 4.021)
INFO  - Loaded configuration from schemaspy.properties
INFO  - Starting schema analysis
INFO  - Connected to SQLite - 3.40.0
INFO  - Gathering schema details
Gathering schema details...(0sec)
```
</details>
```
Connecting relationships...WARN  - No tables or views were found in schema 'dbo'.
ERROR - The schema 'dbo' could not be read/found, schema is specified using the -s option.Make sure user 'null' has the correct privileges to read the schema.Also not that schema names are usually case sensitive.
INFO  - Available schemas(Some of these may be user or system schemas):

ERROR - Unable to determine if any of the schemas contain tables/views
WARN  - Empty schema
null
INFO  - StackTraces have been omitted, use `-debug` when executing SchemaSpy to see them
```


Changing to
```
# db scheme for which generate diagrams
schemaspy.s=null
```

or to (not providing this parameter at all
```
# db scheme for which generate diagrams
# schemaspy.s=dbo
```

were also unsuccessful trials.