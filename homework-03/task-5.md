# Task 5

Напишіть SQL команду, за допомогою якої можна знайти кількість продуктів (рядків) та середню ціну (_price_) у кожного постачальника (_supplier\_id_), та перевірте правильність її виконання в MySQL Workbench.

```sql
SELECT 
    supplier_id,
    COUNT(*) AS order_count,
    FORMAT(ROUND(AVG(price), 2), 2) AS avg_price
FROM
    mydb.products
GROUP BY supplier_id;
```

<figure><img src="../.gitbook/assets/task-5.01.jpg" alt="" width="375"><figcaption></figcaption></figure>
