---
description: >-
  Згрупуйте за іменем категорії, порахуйте кількість рядків у групі, середню
  кількість товару (кількість товару знаходиться в order_details.quantity)
---

# Step 7. Goods by category

```sql
SELECT 
    categories.id AS category_id,
    categories.name AS category_name,
    COUNT(products.id) AS product_count,
    FLOOR(AVG(order_details.quantity)) AS avg_product_count
FROM
    order_details
        INNER JOIN
    products ON order_details.product_id = products.id
        INNER JOIN
    categories ON products.category_id = categories.id
GROUP BY categories.id , categories.name
```

<figure><img src="../.gitbook/assets/hw-04_step-07.webp" alt="" width="375"><figcaption><p>SQL query result</p></figcaption></figure>

#### Результат отриманого запиту

{% embed url="https://docs.google.com/spreadsheets/d/1V3J4GJE19Xq8dgkghTsRODXoED-SImN4uk699pPYw08/edit?usp=sharing" %}
