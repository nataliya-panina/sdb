# Домашнее задание к занятию «Работа с данными (DDL/DML)»


## Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp.

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp.

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос:

ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';  

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

---

## Решение
```
docker run --name sql --rm -p 3306:3306 -e MYSQL_ROOT_PASSWORD=secret -dti mysql:8.0
```
```
USE test;
CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'secret';
SELECT * FROM mysql.user;
```
![image](https://github.com/user-attachments/assets/001864eb-2f41-4c3a-82fc-c53ffce38526)

```
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SHOW GRANTS for 'sys_temp';
```
![image](https://github.com/user-attachments/assets/0a65a25d-dadf-4953-bc69-b49706f5337a)

Дамп не захотел работать,

![image](https://github.com/user-attachments/assets/4c29f0eb-30f2-4e18-889e-7b7fe4bfa318)

Пришлось вручную загружать скрипты:

![image](https://github.com/user-attachments/assets/a649f342-bea9-4b82-9f0c-713c7663538e)


![Простыня](https://github.com/nataliya-panina/sdb/blob/main/6/%D0%9F%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BD%D1%8F)

## Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

Название таблицы | Название первичного ключа
customer         | customer_id

---

## Решение


Name	| Primary key
---|---
payment	| payment_id
rental | rental_id
customer|customer_id
inventory	| inventory_id
store | store_id
staff | staff_id
address	| address_id
city	| city_id
actor	| actor_id
film_actor	| actor_id, film_id
film	| film_id
language	| language_id
film_category	| film_id, category_id
category	| category_id
sales_by_film_category	| category_id
film_text	| film_id
sales_by_store	| sales_by_store_id
actor_info	| actor_id
film_list	| film_id
nicer_but_slower_film_list	| film_id
staff_list	| customer_id
customer_list	| customer_id


---

# Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.



## Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

---

## Решение

---
