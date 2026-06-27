# Домашнее задание к занятию «SQL. Часть 2» Мальцев Андрей

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

## Решение 

```sql
SELECT
    CONCAT(s.last_name, ' ', s.first_name) AS staff_name,
    c.city,
    COUNT(cu.customer_id) AS customer_count
FROM store st
JOIN staff s ON st.manager_staff_id = s.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cu ON st.store_id = cu.store_id
GROUP BY st.store_id, s.last_name, s.first_name, c.city
HAVING COUNT(cu.customer_id) > 300;
```

<img width="2552" height="1387" alt="Задание 1" src="https://github.com/user-attachments/assets/238beee7-7082-4a63-8bfb-7c7ef275d872" />



### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

## Решение 

```sql
SELECT COUNT(*) AS films_longer_than_avg
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

<img width="2493" height="1305" alt="Задание 2" src="https://github.com/user-attachments/assets/9ea57bac-84c1-410a-9ae5-8aeb9e6c3822" />



### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

## Решение 

```sql
SELECT
    DATE_FORMAT(p.payment_date, '%Y-%m') AS month,
    SUM(p.amount) AS total_payment,
    COUNT(r.rental_id) AS rental_count
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
GROUP BY DATE_FORMAT(p.payment_date, '%Y-%m')
ORDER BY total_payment DESC
LIMIT 1;
```

<img width="2552" height="1328" alt="Задание 3" src="https://github.com/user-attachments/assets/1fb0c23e-9747-4ffd-91c4-cda058ce8c7e" />



## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

## Решение

```sql
SELECT CONCAT(s.first_name, ' ', s.last_name) AS Name, COUNT(1) AS Sales,
	CASE
		WHEN COUNT(1) > 8000 THEN 'Yes'
		ELSE 'No'
	END AS Premium
FROM payment p 
JOIN staff s ON p.staff_id = s.staff_id 
GROUP BY p.staff_id;
```

<img width="2559" height="1325" alt="Задание 4" src="https://github.com/user-attachments/assets/a59f4b29-494d-4733-9a16-8f797fbb1714" />
