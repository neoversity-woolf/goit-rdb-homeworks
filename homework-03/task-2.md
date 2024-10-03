# Task 2

Напишіть SQL команду, за допомогою якої можна знайти середнє, максимальне та мінімальне значення стовпчика _price_ таблички `products`_,_ та перевірте правильність її виконання в MySQL Workbench.

```sql
SELECT 
    AVG(price) AS average_price,
    MAX(price) AS max_price,
    MIN(price) AS min_price
FROM
    mydb.products;
```

<figure><img src="../.gitbook/assets/task-2.01.jpg" alt="" width="375"><figcaption></figcaption></figure>
