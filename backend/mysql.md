## db
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| atlas_development  |
| atlas_test         |
| mysql              |
| performance_schema |
+--------------------+
```
### tables
```
mysql> use atlas_development;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-----------------------------+
| Tables_in_atlas_development |
+-----------------------------+
| depots                      |
| schema_migrations           |
| triggers                    |
+-----------------------------+
```
#### depots
```
mysql> select * from depots
    -> ;
+----+------------------------+--------------+---------------------+---------------------+
| id | name                   | organization | created_at          | updated_at          |
+----+------------------------+--------------+---------------------+---------------------+
|  1 | atlas-sanity.atlas.dev | NULL         | 2014-09-01 04:08:15 | 2014-09-01 04:08:15 |
+----+------------------------+--------------+---------------------+---------------------+
```

#### triggers
```
mysql> select * from triggers;
+----+----------+------------------------+------------------------------------------+--------------+---------------------+---------------------+
| id | depot_id | name                   | command                                  | shell_script | created_at          | updated_at          |
+----+----------+------------------------+------------------------------------------+--------------+---------------------+---------------------+
|  1 |        1 | sanity-auth-trigger.sh | Mytrigger change-commit //... "$SCRIPT$" | exit 0       | 2014-09-01 04:08:28 | 2014-09-01 04:08:28 |
+----+----------+------------------------+------------------------------------------+--------------+---------------------+---------------------+
```
