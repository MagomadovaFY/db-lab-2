# Лабораторная работа №2

## Вариант 11

---

### Задача 1. JOIN

**Условие:**  
Вывести список товаров и дату их производства, проданных клиентам из штата 'FL'.

**SQL-запрос:**
```sql
SELECT DISTINCT
    p.model,
    p.production_start_date
FROM sales s
INNER JOIN customers c ON s.customer_id = c.customer_id
INNER JOIN products p ON s.product_id = p.product_id
WHERE c.state = 'FL';

```
**Пояснение:**
INNER JOIN используется, потому что нужны только те продажи, у которых есть и клиент, и товар. Фильтр по штату 'FL' — через WHERE.**
```
```
### Задача 2. Подзапрос / UNION

**Условие:**  
Найти клиентов, у которых нет ни одной покупки.

**SQL-запрос:**
```
```sql
SELECT *
FROM customers c
LEFT JOIN sales s ON c.customer_id = s.customer_id
WHERE s.sales_transaction_date IS NULL;

```
### Пояснение:
LEFT JOIN возвращает всех клиентов, даже если у них нет продаж. Проверка на NULL в поле из sales означает, что записей не найдено.
