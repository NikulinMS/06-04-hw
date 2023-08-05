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

SQL-транзакция:
```
BEGIN;
CREATE TABLE orders_1 (LIKE orders);
INSERT INTO orders_1 SELECT * FROM orders WHERE price >499;
DELETE FROM orders WHERE price >499;
CREATE TABLE orders_2 (LIKE orders);
INSERT INTO orders_2 SELECT * FROM orders WHERE price <=499;
DELETE FROM orders WHERE price <=499;
COMMIT;
```
```
test_database=# SELECT * FROM public.orders;
 id | title | price
----+-------+-------
(0 rows)

test_database=# SELECT * FROM public.orders_1;
 id |       title        | price
----+--------------------+-------
  2 | My little database |   500
  6 | WAL never lies     |   900
  8 | Dbiezdmin          |   501
(3 rows)

test_database=# SELECT * FROM public.orders_2;
 id |        title         | price
----+----------------------+-------
  1 | War and peace        |   100
  3 | Adventure psql time  |   300
  4 | Server gravity falls |   300
  5 | Log gossips          |   123
  7 | Me and my bash-pet   |   499
(5 rows)
```
Да, в PostgreSQL можно было изначально исключить ручное разбиение таблицы orders при проектировании. Для этого необходимо использовать автоматическое разбиение таблиц на разделы (partitions) при создании базы данных или при добавлении новых данных в таблицу.

Автоматическое разбиение таблиц на разделы может быть выполнено при помощи функции partition_by() в SQL-запросе или при помощи оператора CREATE TABLE … PARTITION BY RANGE в языке SQL. При использовании функции partition_by(), можно задать условие для разделения таблицы на разделы по определенному столбцу.

Кроме того, в PostgreSQL также есть возможность автоматического разбиения таблиц при помощи автоматической сегментации (auto-segmentation), которая позволяет автоматически создавать разделы на основе статистики по данным в таблице. Для включения автоматической сегментации таблицы нужно использовать параметр auto_explain в параметре конфигурации сервера.



---

### Задание 4

Создадим бэкап:
```
root@a8c227640926:/# pg_dump -U nikulinm -d test_database > /var/tmp/test_database_dump.sql
```
Уникальность столбца title можно выполнить следующим образом:
```
test_database=# CREATE unique INDEX title_uni ON public.orders(title);
CREATE INDEX
```
Еще один вариант - можно использовать функцию ALTER TABLE и добавить ограничение уникальности для этого столбца.

Пример:

ALTER TABLE test_database ADD CONSTRAINT unique_title UNIQUE (title);

После этого в таблице test_database будет добавлено ограничение уникальности на столбец title, и значения этого столбца не могут повторяться.

---

