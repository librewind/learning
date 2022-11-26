# Learning

## Table of Contents

### DB

* [Transaction Isolation Levels](DB/transactions.md)
* [Locks](DB/locks.md)
* [EXPLAIN](DB/explain.md)

### GoF

* [Singleton](GoF/singleton.md)

## Useful commands

Start services:
```console 
foo@bar:~$ docker-compose up -d
```
Connect to mysql service console:
```console
foo@bar:~$ docker-compose exec mysql sh
sh-4.4$
```

Connect to postgres service console:
```console            
foo@bar:~$ docker-compose exec postgres sh
#
``` 
Connect to MySQL:
```console
sh-4.4$ mysql -h 127.0.0.1 -u root -pdev
mysql>
```

Connect to PostgreSQL:
```console
# psql -U dev
psql (14.6 (Debian 14.6-1.pgdg110+1))
Type "help" for help.

dev=#
```

Create test database and select it (MySQL):
```sql
CREATE DATABASE test;
USE test;
```

Create test database and select it (PostgreSQL):                                                                                                                                                             
```sql
CREATE DATABASE test;
\c test
```

