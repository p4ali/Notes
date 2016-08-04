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
\dp # inspect the privilege
\i # run sql file
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

## Run query from commandline

```bash
psql -Upostgres -draymond ‘select usernam, email, company_name from users’ >> a.out
```

Which similar to ruby console query
```
rvm (default|helix) do bundle exec rails c
@> puts User.pluck(:username, :email, :company_name).select{|a| a[1] !~ /perforce.com/}.reduce('') {|s,a| s+"\n"+a.join(' ‘)}
```

## Reference
* [ZetCode has a great postgresql JDBC tutorial](http://zetcode.com/db/postgresqljavatutorial/)
