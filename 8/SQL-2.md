# Домашнее задание к занятию «SQL. Часть 2»
Задание можно выполнить как в любом IDE, так и в командной строке.

---
## Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;  
город нахождения магазина;   
количество пользователей, закреплённых в этом магазине.  

---
## Решение

```sql
SELECT st.store_id, CONCAT(stf.first_name, ' ', stf.last_name) AS manager, ct.city, cs.nc as customers
FROM store st
JOIN (SELECT store_id, COUNT(*) nc
FROM customer
GROUP BY store_id
HAVING nc > 300) as cs ON st.store_id = cs.store_id
JOIN staff stf ON st.manager_staff_id = stf.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city ct ON a.city_id = ct.city_id
;
```
![image](https://github.com/user-attachments/assets/44d79fe4-398d-4785-bbec-3fa8e6e8effc)


---

## Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

---
## Решение

```sql
SELECT COUNT(*) 
FROM film
WHERE (SELECT avg(f.length) as average
FROM film f) < film.length;
```

![image](https://github.com/user-attachments/assets/4018cd1c-5646-4b83-b4ff-7ce36bf2e677)

---

## Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

---
## Решение

```sql
SELECT gr.best_month, amount, gr.rents
FROM(SELECT SUM(amount) amount, MONTH(payment_date) mon
FROM payment 
GROUP BY mon) as a
JOIN
(SELECT MONTH(rental_date) best_month, COUNT(*) rents
FROM rental r
GROUP BY best_month) gr ON a.mon = gr.best_month
ORDER BY amount DESC
LIMIT 1;
```
![image](https://github.com/user-attachments/assets/650a6897-9b50-44aa-83a0-d14330c91e5c)

---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

---
## Задание 4*
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

---
## Решение
```sql
SELECT	staff_id, COUNT(*) rents,
	CASE
		WHEN COUNT(*) > 8000 THEN "YES!!"
		ELSE "no"
	END AS prime_or_not
FROM
	rental
GROUP BY
	staff_id;
```
![image](https://github.com/user-attachments/assets/2cdaf95f-95b5-4dad-971c-8e371e008ef0)

---

## Задание 5*
Найдите фильмы, которые ни разу не брали в аренду.

---
