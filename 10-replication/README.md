# Домашнее задание к занятиям «Репликация и масштабирование»
---

## Задание 1

На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.

Ответить в свободной форме.  

## Решение

Репликация master-slave обеспечивает отказоустойчивость - пока master занимается вычислениями и записью данных, slave может использоваться для чтения данных из БД, таким образом уменьшая нагрузку на master. В случае отказа master, slave переключается в режим master.

Репликация master-master - это избыточная конфигурация, при которой два или более master-сервера могут работать на чтение и запись данных в БД, при этом синхронизируясь между собой. При выходе из строя одного из серверов, другие берут на себя его нагрузку. 

---
## Задание 2

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.


## Решение

см. файл compose.yml  

```bash
docker compose up -d
```
Конфигурация мастер-сервера:  

```bash
docker exec -it replication-master bash
bash-4.4# mysql -uroot -p
```
**on replication-master:**  
```sql
ALTER USER 'replication_user'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'replication_password';
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;
```
![img](https://github.com/nataliya-panina/sdb/blob/master/10-replication/img/position_master.png)

### Конфигурация ведомого сервера:  
```bash
docker exec -it replication-slave bash
bash-4.4# mysql -uroot -p
```
**on replication-slave:**  
```sql
CHANGE MASTER TO
    ->   MASTER_HOST='replication-master',
    ->   MASTER_USER='replication_user',
    ->   MASTER_PASSWORD='replication_password',
    ->   MASTER_LOG_FILE='mysql-bin.000003',
    ->   MASTER_LOG_POS=851;

START SLAVE;
SHOW SLAVE STATUS\G
```
.[img](https://github.com/nataliya-panina/sdb/blob/master/10-replication/img/show_slave_status.png)

Проверка:  

**on replication-master:**  
```sql
use mydb;
create table user (id int);
insert into user values (1);
select * from user;
```
**on replication-slave:**  
```sql
use mydb;
select * from user;
```
![img](https://github.com/nataliya-panina/sdb/blob/master/10-replication/img/master_slave.png)
[шпаргалка](https://dev.to/siddhantkcode/how-to-set-up-a-mysql-master-slave-replication-in-docker-4n0a)
---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

## Задание 3*
Выполните конфигурацию master-master репликации. Произведите проверку.

Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.