---
description: Відсортуйте рядки за спаданням кількості рядків.
---

# Step 9. Sorting goods

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
HAVING avg_product_count > 21
ORDER BY product_count DESC;
```

<figure><img src="../.gitbook/assets/hw-04_step-09.webp" alt="" width="375"><figcaption><p>SQL query result</p></figcaption></figure>

#### Результат отриманого запиту

{% embed url="https://docs.google.com/spreadsheets/d/1Sw_ysnJ36OnBDEaEZHYVOdrR0HrNGoVtnNsk0P3bxhY/edit?usp=sharing" %}
