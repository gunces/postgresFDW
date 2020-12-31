# Setup postgres_fdw

Postgres has many Foreign Data Wrappers(FDW). It uses for getting remote data in a Postgres table. For more information visit [postgresql.org](https://www.postgresql.org/docs/current/postgres-fdw.html, "postgres_fdw") 

# How to install postgres_fdw?

There are few steps for installation postgres_fdw.

1. CREATE EXTENSION
2. CREATE SERVER
3. CREATE USER MAPPING
4. IMPORT FOREIGN TABLE


## CREATE EXTENSION

First, create postgres_fdw extension on database which you want to store FDW tables.

````
CREATE EXTENSION postgres_fdw;
````

## CREATE SERVER

Give connection information to Postgres as which instance you want to connect or which database you want to reach? Input remote Postgres's connection information into OPTIONS

````
CREATE SERVER <server name>
        FOREIGN DATA WRAPPER postgres_fdw
        OPTIONS (host '<ip address>', port '<d port>', dbname '<db name>');
````

## CREATE USER MAPPING

I created SERVER but which remote user can connect I did not specify yet. 

````
CREATE USER MAPPING FOR <local user name>
        SERVER <server name>
        OPTIONS (user '<remote user name>', password '<remote user's password>');
````

## CREATE FOREIGN TABLE/IMPORT FOREIGN SCHEMA

This is not necessary but I would like to prefer create new schema for foreign tables. Otherwise you create you foreign tables into existed schema. 

````
CREATE SCHEMA <local schema> ;
````

You can import a table or all tables under a specific schema. 

### Create foreign table

````
CREATE FOREIGN TABLE <foreign table name> (col1 data type, col2 data type...)
SERVER <server name>
OPTIONS (schema_name '<remote schema>', table_name '<remote table>');
````

### Import foreign schema

````
IMPORT FOREIGN SCHEMA <remote schema name> 
    FROM SERVER <server name> INTO <local schema>;
````

>DDL structure is not transfer from remote to local side. If your table has new column, you should re create your foreign table.


## Drop objects

````
/*	
DROP SERVER <server name>;
DROP USER MAPPING FOR <local user> SERVER <server name>;
DROP FOREIGN TABLE <foreign table name>;
*/
````
