---
description: Відфільтруйте рядки, де середня кількість товару більша за 21.
---

# Step 8. Filtering goods

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
HAVING avg_product_count > 21;
```

<figure><img src="../.gitbook/assets/hw-04_step-08.webp" alt="" width="375"><figcaption><p>SQL query result</p></figcaption></figure>

#### Результат отриманого запиту

{% embed url="https://docs.google.com/spreadsheets/d/15TN6N4om2rXWcDmodc44fP6w9o-s4-UjppnXecwkMkY/edit?usp=sharing" %}
