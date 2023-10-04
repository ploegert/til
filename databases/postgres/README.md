# postgres

[https://cheatography.com/tme520/cheat-sheets/postgresql/](https://cheatography.com/tme520/cheat-sheets/postgresql/)

## Fundamentals <a href="#title_3884_12308" id="title_3884_12308"></a>

| **View PostgreSQL server version**postgres=# SELECT version();                                                        |
| --------------------------------------------------------------------------------------------------------------------- |
| **View PostgreSQL client version**psql -V                                                                             |
| **Check if Postgres is running**/etc/init.d/postgresql status                                                         |
| **Start/Stop/Restart Postgres (SysVinit)**/etc/init.d/postgresql start \| stop \| restart                             |
| **Start/Stop/Restart Postgres (Systemd)**systemctl start \| stop \| restart postgresql.service                        |
| **Enter our queries in our favorite editor**postgres=# \e                                                             |
| **Change PostgreSQL root password**psql postgres postgres                                                             |
|  postgres=# ALTER ROLE postgres WITH PASSWORD 'toto';                                                                 |
|  postgres=# \q                                                                                                        |
| **List available functions**postgres=# \df                                                                            |
| **View Help**postgres=# \\?                                                                                           |
|  postgres=# \h CREATE                                                                                                 |
|  postgres=# \h CREATE INDEX                                                                                           |
| **List DBs**postgres=# \l                                                                                             |
| **Create a DB from Postgres**postgres=# CREATE DATABASE maDB WITH OWNER akiko;                                        |
| **Create a DB from the Shell**postgres@debianVM:\~$ /usr/bin/createdb maDB -O midori                                  |
| **Delete a database from Postgres**postgres=# DROP DATABASE maDB;                                                     |
| **Delete a database from the Shell**postgres@debianVM:\~$ dropdb maDB                                                 |
| **List the tables of a DB**postgres-# \c postgres                                                                     |
|  postgres-# \d                                                                                                        |
| **Get a list of data types available in PostgreSQL**postgres=# SELECT typname,typlen from pg\_type where typtype='b'; |
| **Redirect query results to a file**postgres=# \o nom\_fichier (enables redirection)                                  |
|  postgres=# \o (cancels redirect)                                                                                     |

## Backups <a href="#title_3884_12310" id="title_3884_12310"></a>

| **Back up a single database**pg\_dump -U user nom\_DB -f madb.sql                                                                   |
| ----------------------------------------------------------------------------------------------------------------------------------- |
| **Back up all databases**pg\_dumpall > all.sql                                                                                      |
| **Back up global objects (users + tablespaces)**pg\_dumpall -g > everything.sql                                                     |
| **Back up a table**pg\_dump --table articles -U midori nom\_DB -f unetable.sql                                                      |
| **Restore a database**psql -U midori -d nom\_DB -f madb.sql                                                                         |
| **Restore all databases**psql -f all.sql                                                                                            |
| **Restore a table**psql -f a table.sql nom\_DB                                                                                      |
| **Back up a local database and restore it to a remote server in one line**pg\_dump nom\_DB\_source \| psql -h server nom\_DB\_cible |

## Index <a href="#title_3884_12312" id="title_3884_12312"></a>

| **See the indexes of a table**postgres=# \d nom\_table                                 |
| -------------------------------------------------------------------------------------- |
| **Creating an index on a table**CREATE INDEX name ON table USING type\_index (column); |

Postgres supports 5 types of indexes: Balanced-Tree (btree; used by default), hash (unlogged transactions, deprecated), Generalized Search Tree (gist), Generalized Inverted Indexes (gin) and Space-Partitioned GIST (spgist).

## Investigate, analyze <a href="#title_3884_12314" id="title_3884_12314"></a>

| **View the history file**cat \~/.psql\_history                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------------- |
| **Enable/disable timing**postgres=# \timing                                                                                      |
| **Get execution details of a query without running it**postgres=# EXPLAIN SELECT typname,typlen from pg\_type where typtype='b'; |
| **Get statistics for a query that just turned**postgres=# EXPLAIN ANALYSE SELECT typname,typlen from pg\_type where typtype='b'; |

A bit like Linux with its _bash\_history_, PostgreSQL keeps a list of commands executed in the _$HOME/.psql\_history_ file.\
\
Timing saves the time it takes a query to execute.



## Users and roles <a href="#title_3884_12309" id="title_3884_12309"></a>

| **Create a user from the Shell**/usr/bin/createuser midori                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Create a user from Postgres**postgres=# CREATE USER akiko WITH password 'yamete';                                                                       |
| **Grant privileges to a user**postgres=# GRANT ALL PRIVILEGES ON DATABASE CityHunter TO akiko;                                                            |
| **Delete a user from the Shell**/usr/bin/dropuser midori                                                                                                  |
| **Delete a user from Postgres**postgres=# DROP USER akiko;                                                                                                |
| **List roles**postgres=# \du                                                                                                                              |
| **Create a role**postgres=# CREATE ROLE trainer WITH LOGIN ENCRYPTED PASSWORD 'learn' CREATEDB;                                                           |
| **Create a role with multiple privileges**postgres=# CREATE ROLE demidieu WITH LOGIN ENCRYPTED PASSWORD 'toto' CREATEDB CREATEROLE REPLICATION SUPERUSER; |
| **Edit a role**postgres=# ALTER ROLE demidieu CREATEROLE CREATEDB REPLICATION SUPERUSER;                                                                  |
| **Delete a role**postgres=# DROP ROLE trainer;                                                                                                            |

These two variants require to be a postgres user (su - _postgres_).

## Disk space <a href="#title_3884_12311" id="title_3884_12311"></a>

| **Calculate the disk space occupied by a database**postgres=# SELECT pg\_database\_size('foodb');                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  postgres=# SELECT pg\_size\_pretty(pg\_database\_size('foodb'));                                                                                                                                                                                                                                                                                                 |
| **Calculate disk space occupied by a table (including indexes)**postgres=# SELECT pg\_size\_pretty(pg\_total\_relation\_size('grosse\_table'));                                                                                                                                                                                                                   |
| **Calculate the disk space occupied by a table (without the index)**postgres=# SELECT pg\_size\_pretty(pg\_relation\_size('grosse\_table'));                                                                                                                                                                                                                      |
| **Find the largest table** (variant 1)postgres=# SELECT relname, relpages FROM pg\_class ORDER BY relpages DESC;                                                                                                                                                                                                                                                  |
| **Find the largest table** (variant 2)postgres=# SELECT nspname \|\| '.' \|\| relname AS tablename, pg\_size\_pretty(pg\_table\_size((nspname \|\| '.' \|\| relname)::regclass)) AS size FROM pg\_class c JOIN pg\_namespace n ON (c.relnamespace = n.oid) WHERE relkind = 'r' ORDER BY pg\_table\_size((nspname \|\| '.' \|\| relname)::regclass) DESC LIMIT 10; |

## Transactions <a href="#title_3884_12313" id="title_3884_12313"></a>

| **Start a transaction**postgres=# BEGIN                 |
| ------------------------------------------------------- |
| **Make a rollback**postgres=# ROLLBACK                  |
| **Make a commit** (end of transaction)postgres=# COMMIT |

## .SQL <a href="#title_3884_12315" id="title_3884_12315"></a>

| **Count rows in a table**postgres=# select count(\*) from table;                                                                                    |
| --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Generate a series of numbers and insert it into a table**postgres=# INSERT INTO numbers (num) VALUES ( generate\_series(1,100));                  |
| **Retrieve the second smallest value in a column**postgres=# SELECT MIN(num) from number\_table WHERE num > ( SELECT MIN(num) FROM number\_table ); |
| **Retrieve the second largest value in a column**postgres=# SELECT MAX(num) FROM number\_table WHERE num < ( SELECT MAX(num) FROM number\_table );  |
| **Encrypt, then save a password**postgres=# SELECT crypt ( 'midori', gen\_salt('md5') );                                                            |
