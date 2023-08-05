# Домашнее задание к занятию "`PostgreSQL`" - `Никулин Михаил Сергеевич`



---

### Задание 1

- вывод списка БД

```
nikulinm_db=# \l
                                  List of databases
    Name     |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-------------+----------+----------+------------+------------+-----------------------
 nikulinm_db | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres    | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 |
 template0   | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 | =c/nikulinm          +
             |          |          |            |            | nikulinm=CTc/nikulinm
 template1   | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 | =c/nikulinm          +
             |          |          |            |            | nikulinm=CTc/nikulinm
(4 rows)
```

- подключение к БД
```
nikulinm_db=# \c postgres
You are now connected to database "postgres" as user "nikulinm".
```

- вывод списка таблиц  
```
postgres=# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | nikulinm
 pg_catalog | pg_am                   | table | nikulinm
 pg_catalog | pg_amop                 | table | nikulinm
 pg_catalog | pg_amproc               | table | nikulinm
 pg_catalog | pg_attrdef              | table | nikulinm
 pg_catalog | pg_attribute            | table | nikulinm
 pg_catalog | pg_auth_members         | table | nikulinm
 pg_catalog | pg_authid               | table | nikulinm
 pg_catalog | pg_cast                 | table | nikulinm
 pg_catalog | pg_class                | table | nikulinm
 pg_catalog | pg_collation            | table | nikulinm
 pg_catalog | pg_constraint           | table | nikulinm
 pg_catalog | pg_conversion           | table | nikulinm
 ...
```

- вывод описания содержимого таблиц 
```
postgres=# \dS+ pg_aggregate
                                   Table "pg_catalog.pg_aggregate"
      Column      |   Type   | Collation | Nullable | Default | Storage  | Stats target | Description
------------------+----------+-----------+----------+---------+----------+--------------+-------------
 aggfnoid         | regproc  |           | not null |         | plain    |              |
 aggkind          | "char"   |           | not null |         | plain    |              |
 aggnumdirectargs | smallint |           | not null |         | plain    |              |
 aggtransfn       | regproc  |           | not null |         | plain    |              |
 aggfinalfn       | regproc  |           | not null |         | plain    |              |
 aggcombinefn     | regproc  |           | not null |         | plain    |              |
 aggserialfn      | regproc  |           | not null |         | plain    |              |
 aggdeserialfn    | regproc  |           | not null |         | plain    |              |
 aggmtransfn      | regproc  |           | not null |         | plain    |              |
 aggminvtransfn   | regproc  |           | not null |         | plain    |              |
 aggmfinalfn      | regproc  |           | not null |         | plain    |              |
 aggfinalextra    | boolean  |           | not null |         | plain    |              |
 aggmfinalextra   | boolean  |           | not null |         | plain    |              |
 aggfinalmodify   | "char"   |           | not null |         | plain    |              |
 aggmfinalmodify  | "char"   |           | not null |         | plain    |              |
 aggsortop        | oid      |           | not null |         | plain    |              |
 aggtranstype     | oid      |           | not null |         | plain    |              |
 aggtransspace    | integer  |           | not null |         | plain    |              |
 aggmtranstype    | oid      |           | not null |         | plain    |              |
 aggmtransspace   | integer  |           | not null |         | plain    |              |
 agginitval       | text     | C         |          |         | extended |              |
 aggminitval      | text     | C         |          |         | extended |              |
Indexes:
    "pg_aggregate_fnoid_index" UNIQUE, btree (aggfnoid)
Access method: heap
```

- выход из psql
```
postgres=# \q
```

---

### Задание 2

Создадим БД test_database:
```
nikulinm_db=# create database test_database;
CREATE DATABASE
nikulinm_db=# \l
                                   List of databases
     Name      |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
---------------+----------+----------+------------+------------+-----------------------
 nikulinm_db   | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres      | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 |
 template0     | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 | =c/nikulinm          +
               |          |          |            |            | nikulinm=CTc/nikulinm
 template1     | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 | =c/nikulinm          +
               |          |          |            |            | nikulinm=CTc/nikulinm
 test_database | nikulinm | UTF8     | en_US.utf8 | en_US.utf8 |
(5 rows)
```
Восстановим бэкап:
```
root@a8c227640926:/# psql -U nikulinm -d test_database < /var/tmp/test_dump.sql
SET
SET
SET
SET
SET
 set_config
------------

(1 row)

SET
SET
SET
SET
SET
SET
CREATE TABLE
ERROR:  role "postgres" does not exist
CREATE SEQUENCE
ERROR:  role "postgres" does not exist
ALTER SEQUENCE
ALTER TABLE
COPY 8
 setval
--------
      8
(1 row)

ALTER TABLE
```
Подключимся к БД test_database, посмотрим список существующих таблиц и соберем статистику с помощью ANALYZE:
```
root@a8c227640926:/# psql -U nikulinm -d test_database
psql (13.11 (Debian 13.11-1.pgdg120+1))
Type "help" for help.

test_database=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner
--------+--------+-------+----------
 public | orders | table | nikulinm
(1 row)

test_database=# ANALYZE VERBOSE orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
```
Найдем столбец таблицы orders с наибольшим средним значением размера элементов в байтах (это title):
```
test_database=# select attname, avg_width from pg_stats where tablename='orders';
 attname | avg_width
---------+-----------
 id      |         4
 title   |        16
 price   |         4
(3 rows)
```

---

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

---
## Дополнительные задания (со звездочкой*)


### Задание 5

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
