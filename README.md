# Лабораторная работа №2

### Задание 2.1. Анализ продаж (JOIN)

**Условие:**  
Список клиентов, купивших автомобиль (тип продукта 'automobile'), у которых указан телефон.

**SQL-запрос:**
```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    c.phone
FROM sales s
INNER JOIN customers c ON s.customer_id = c.customer_id
INNER JOIN products p ON s.product_id = p.product_id
WHERE p.product_type = 'automobile'
  AND c.phone IS NOT NULL;
```
**Пояснение:**
INNER JOIN используется, так как нужны только те продажи, где есть и клиент, и товар. Условия фильтрации — по типу товара и наличию телефона.
```
```
### Задание 2.2. Список гостей (UNION)
**Условие:**
Составить единый список гостей для вечеринки в Лос-Анджелесе: клиенты из города 'Los Angeles' и сотрудники, работающие в дилерских центрах в 'Los Angeles'.

**SQL-запрос:**
```sql
SELECT
    first_name,
    last_name,
    'Customer' AS guest_type
FROM customers
WHERE city = 'Los Angeles'

UNION

SELECT
    sp.first_name,
    sp.last_name,
    'Employee' AS guest_type
FROM salespeople sp
INNER JOIN dealerships d ON sp.dealership_id = d.dealership_id
WHERE d.city = 'Los Angeles';
```
## Задание 2.3. Подготовка витрины данных
**Условие:**
Объединить таблицы sales, customers, products, dealerships (LEFT JOIN для dealerships).

Вывести все столбцы customers и products

Заменить NULL в dealership_id на -1 (COALESCE)

Добавить high_savings: 1, если sales_amount ≤ base_msrp - 500, иначе 0

**SQL-запрос:**
```sql
SELECT
    c.*,
    p.*,
    COALESCE(s.dealership_id, -1) AS dealership_id_fixed,
    CASE
        WHEN s.sales_amount <= p.base_msrp - 500 THEN 1
        ELSE 0
    END AS high_savings
FROM sales s
LEFT JOIN customers c ON s.customer_id = c.customer_id
LEFT JOIN products p ON s.product_id = p.product_id
LEFT JOIN dealerships d ON s.dealership_id = d.dealership_id;
```

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
**Пояснение:**
LEFT JOIN возвращает всех клиентов, даже если у них нет продаж. Проверка на NULL в поле из sales означает, что записей не найдено.
```
```
### Задача 3. Преобразование данных
**Условие:**
Создать категорию длительности найма продавца: Junior (< 2 лет), Senior (≥ 2 лет).

**SQL-запрос:**
```sql
SELECT
    first_name,
    last_name,
    hire_date,
    CASE
        WHEN EXTRACT(YEAR FROM AGE(CURRENT_DATE, hire_date)) < 2 THEN 'Junior'
        ELSE 'Senior'
    END AS experience_level
FROM salespeople;
```
**Пояснение:**
AGE(CURRENT_DATE, hire_date) вычисляет интервал между сегодняшней датой и датой найма. EXTRACT(YEAR FROM ...) берёт количество полных лет.
