# Домашнее задание к занятиям «Репликация и масштабирование»
---
## Задание 1
Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.


## Решение

см. файл compose.yml  

```bash
docker compose up -d
```

Конфигурация мастера:  
```bash
docker exec -it replication-master bash
bash-4.4# mysql -uroot -p
```

```sql
mysql> ALTER USER 'replication_user'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'replication_password';
er'@'%';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;Query OK, 0 rows affected (0.01 sec)

mysql> GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000003 |      851 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set, 1 warning (0.00 sec)
```
Конфигурация ведомого сервера:  
```bash
docker exec -it replication-slave bash
bash-4.4# mysql -uroot -p
```
on replication-slave:
```sql
CHANGE MASTER TO
    ->   MASTER_HOST='replication-master',
    ->   MASTER_USER='replication_user',
    ->   MASTER_PASSWORD='replication_password',
    ->   MASTER_LOG_FILE='mysql-bin.000003',
    ->   MASTER_LOG_POS=851;

START SLAVE;
```
Проверка:  

on replication-master:  
```sql
use my_db;
create table user (id int);
insert into user values (1);
select * from user;
```
on replication-slave:  
```sql
use my_db;
select * from user;
```
[шпаргалка](https://dev.to/siddhantkcode/how-to-set-up-a-mysql-master-slave-replication-in-docker-4n0a)
----

## Задание 2

Разработайте план для выполнения горизонтального и вертикального шаринга базы данных. База данных состоит из трёх таблиц:

- пользователи,
- книги,
- магазины (столбцы произвольно).  
Опишите принципы построения системы и их разграничение или разбивку между базами данных.

Пришлите блоксхему, где и что будет располагаться. Опишите, в каких режимах будут работать сервера.


## Решение


----
## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

## Задание 3*
Опишите основные преимущества использования масштабирования методами:

активный master-сервер и пассивный репликационный slave-сервер;
master-сервер и несколько slave-серверов;
активный сервер со специальным механизмом репликации — distributed replicated block device (DRBD);
SAN-кластер.
Дайте ответ в свободной форме.


## Решение


----
## Задание 4*
Выполните настройку выбранных методов шардинга из задания 2.

Пришлите конфиг Docker и SQL скрипт с командами для базы данных.


## Решение


----
