r
# Домашнее задание к занятию «SQL. Часть 1»
------
## Задание 1
Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

---
## Решение
```sql

USE sakila;
SELECT DISTINCT district FROM address
WHERE district LIKE 'K%a' AND district NOT LIKE '% %';
```
![image](https://github.com/user-attachments/assets/9a20a836-f850-4df8-a9e3-aff13683df13)

---

## Задание 2
Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно и стоимость которых превышает 10.00.

---
## Решение
```sql
SELECT DATE(payment_date), amount FROM payment
WHERE amount > 10.00
AND DATE(payment_date) BETWEEN '2005-06-15' AND '2005-06-18';
```

![image](https://github.com/user-attachments/assets/76854fdd-be1a-450d-b306-7883bb7af10f)


---

## Задание 3
Получите последние пять аренд фильмов.

---
## Решение
```sql
USE sakila;
SELECT * FROM rental
ORDER BY rental_date DESC
LIMIT 5;
```
![image](https://github.com/user-attachments/assets/03795284-ff60-4666-8839-697614b0c588)

---

## Задание 4
Одним запросом получите активных покупателей, имена которых Kelly или Willie.

Сформируйте вывод в результат таким образом:

все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
замените буквы 'll' в именах на 'pp'.

---
## Решение

```sql
select customer_id, LOWER(REPLACE(first_name, 'LL', 'PP')),LOWER(last_name) from customer
where first_name='Kelly' OR first_name='Willie'
AND active='1';
```

![image](https://github.com/user-attachments/assets/46c05170-36d9-4599-a6b6-7fff2e5f168b)

---
# Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.


## Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.

---
## Решение
```sql
select LEFT(email, position('@' IN email)-1) as fio,
       RIGHT(email, position('@' IN email)+1) as domain
FROM customer
LIMIT 20;
```
![image](https://github.com/user-attachments/assets/d3a9ea6e-153b-4102-a76b-4cee126dba2e)

---

## Задание 6*
Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.

---
## Решение



---
