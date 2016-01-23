## [url](https://jdbc.postgresql.org/documentation/80/connect.html)

```
jdbc:postgresql://192.168.33.10:5432/raymond_development 
```

## Command line
```bash
psql -Upostgres -draymond_development -h192.168.33.10 -p5432
\list # list databases
\connect raymond_test
\dt # list table
\q # quit
\? # help command
```

## SQL

SQL query must end with **;**. 

```sql
SELECT * FROM table_name;
```

## config

To make posgresql accessible from other host:

* allow access from any host 
```bash
echo "listen_addresses = '*'" >> /etc/postgresql/9.3/main/postgresql.conf
```
* allow access without password
```bash
echo "host    all     all     192.168.0.1/32  trust" >> /etc/postgresql/9.3/main/pg_hba.conf
```
