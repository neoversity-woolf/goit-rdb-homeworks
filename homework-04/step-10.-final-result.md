---
description: Виведіть на екран (оберіть) чотири рядки з пропущеним першим рядком
---

# Step 10. Final result

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
ORDER BY product_count DESC
LIMIT 4 OFFSET 1;
```

<figure><img src="../.gitbook/assets/hw-04_step-10.jpg" alt="" width="375"><figcaption><p>SQL query result</p></figcaption></figure>

#### Результат отриманого запиту

{% embed url="https://docs.google.com/spreadsheets/d/1j04p4DAOhKwxlmKHhYcqoqSvtLUxJnbTJD5H-Y6x-M4/edit?usp=sharing" %}
