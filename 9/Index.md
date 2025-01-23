# Домашнее задание к занятию «Индексы»
------

## Задание 1
---

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

---
## Решение

```sql
SELECT SUM(INDEX_LENGTH)/SUM(DATA_LENGTH)*100 
FROM information_schema.TABLES
;
```
![image](https://github.com/user-attachments/assets/0ef8f93a-2109-47e2-9400-dfafb99f675f)

---
## Задание 2
---

Выполните explain analyze следующего запроса:

```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

---
## Решение
```sql
EXPLAIN ANALYZE
select distinct concat(c.last_name, ' ', c.first_name), -- Поиск уникальных фио
sum(p.amount) over (partition by c.customer_id, f.title) -- И их платежей с группировкой по названию фильма - > Таблица film избыточна, в таблице rental содержится инфо о фильме в виде inventory_id
from payment p, rental r, customer c, inventory i, film f -- Из всех этих таблиц
where date(p.payment_date) = '2005-07-30' -- На конкретную дату
and p.payment_date = r.rental_date -- При этом Дата платежа = дате аренды, при JOIN можно связать по rental_id (индекс)
and r.customer_id = c.customer_id -- Отбор по клиенту > Можно сделать JOIN клиента с его арендой по индексам customer_id
and i.inventory_id = r.inventory_id; -- таблица inventory избыточна, сгруппировать можно по invertory_id из таблицы rental
```
Выполнение EXPLAIN ANALYZE:
```
-> Table scan on <temporary>  (cost=2.5..2.5 rows=0) (actual time=9119..9119 rows=391 loops=1)
    -> Temporary table with deduplication  (cost=0..0 rows=0) (actual time=9119..9119 rows=391 loops=1)
        -> Window aggregate with buffering: sum(payment.amount) OVER (PARTITION BY c.customer_id,f.title )   (actual time=4041..8798 rows=642000 loops=1)
            -> Sort: c.customer_id, f.title  (actual time=4041..4166 rows=642000 loops=1)
                -> Stream results  (cost=10.1e+6 rows=15.6e+6) (actual time=1.14..2906 rows=642000 loops=1)
                    -> Nested loop inner join  (cost=10.1e+6 rows=15.6e+6) (actual time=1.12..2495 rows=642000 loops=1)
                        -> Nested loop inner join  (cost=8.51e+6 rows=15.6e+6) (actual time=1.11..2185 rows=642000 loops=1)
                            -> Nested loop inner join  (cost=6.95e+6 rows=15.6e+6) (actual time=1.1..1847 rows=642000 loops=1)
                                -> Inner hash join (no condition)  (cost=1.54e+6 rows=15.4e+6) (actual time=1.07..90.4 rows=634000 loops=1)
                                    -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1.61 rows=15400) (actual time=0.462..15.5 rows=634 loops=1)
                                        -> Table scan on p  (cost=1.61 rows=15400) (actual time=0.441..9.5 rows=16044 loops=1)
                                    -> Hash
                                        -> Covering index scan on f using idx_title  (cost=103 rows=1000) (actual time=0.0723..0.482 rows=1000 loops=1)
                                -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.25 rows=1.01) (actual time=0.00179..0.00253 rows=1.01 loops=634000)
                            -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=250e-6 rows=1) (actual time=257e-6..296e-6 rows=1 loops=642000)
                        -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=250e-6 rows=1) (actual time=216e-6..256e-6 rows=1 loops=642000)
```

Из шести таблиц можно оставить три:	  

![image](https://github.com/user-attachments/assets/8ecfe17b-bf0a-4a3f-8001-beb84890ea0b)


---
Дополнительные задания (со звёздочкой*)
---
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

## Задание 3*
---
Самостоятельно изучите, какие типы индексов используются в PostgreSQL. Перечислите те индексы, которые используются в PostgreSQL, а в MySQL — нет.

Приведите ответ в свободной форме.

---
## Решение


---
