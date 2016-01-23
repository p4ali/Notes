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

