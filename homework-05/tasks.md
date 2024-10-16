---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Tasks

1\. Напишіть SQL запит, який буде відображати таблицю `order_details` та поле `customer_id` з таблиці `orders` відповідно для кожного поля запису з таблиці `order_details`.

Це має бути зроблено за допомогою вкладеного запиту в операторі `SELECT`.

```sql
USE lesson_03;

SELECT 
    *,
    (SELECT 
            customer_id
        FROM
            orders
        WHERE
            id = order_details.order_id) AS customer_id
FROM
    order_details;
```

<figure><img src="../.gitbook/assets/hw-05_task_01.webp" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="https://docs.google.com/spreadsheets/d/1AHsTrrfsuotJ9VCcOQmKUBah8ull2a_A9dxi9XkjJkE/edit?usp=sharing" %}
task-01
{% endembed %}

2\. Напишіть SQL запит, який буде відображати таблицю `order_details`. Відфільтруйте результати так, щоб відповідний запис із таблиці `orders` виконував умову `shipper_id=3`.

Це має бути зроблено за допомогою вкладеного запиту в операторі `WHERE`.

```sql
SELECT 
    *
FROM
    order_details
WHERE
    order_id IN (SELECT 
            id
        FROM
            orders
        WHERE
            shipper_id = 3);
```

<figure><img src="../.gitbook/assets/hw-05_task_02.webp" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="https://docs.google.com/spreadsheets/d/166-uR73_Uy_wYqGBYM9SCJv9K7eBG8o37qmOanvwXyw/edit?usp=sharing" %}
task-02
{% endembed %}

3\. Напишіть SQL запит, вкладений в операторі `FROM`, який буде обирати рядки з умовою `quantity>10` з таблиці `order_details`. Для отриманих даних знайдіть середнє значення поля `quantity` — групувати слід за `order_id`.

```sql
SELECT 
    t.order_id, AVG(t.quantity) AS avg_quantity
FROM
    (SELECT 
        *
    FROM
        order_details
    WHERE
        quantity > 10) AS t
GROUP BY order_id;
```

<figure><img src="../.gitbook/assets/hw-05_task_03.webp" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="https://docs.google.com/spreadsheets/d/1Cqt9MFEKactxWYh-3ZCWGUPpbz6_cj_Jnus8RBc3lp4/edit?usp=sharing" %}
task-03
{% endembed %}

4\. Розв’яжіть завдання 3, використовуючи оператор `WITH` для створення тимчасової таблиці `temp`. Якщо ваша версія MySQL більш рання, ніж 8.0, створіть цей запит за аналогією до того, як це зроблено в конспекті.

```sql
WITH t AS (
SELECT 
        *
    FROM
        order_details
    WHERE
        quantity > 10
)
SELECT 
    t.order_id, AVG(t.quantity) AS avg_quantity
FROM t
GROUP BY order_id;
```

<figure><img src="../.gitbook/assets/hw-05_task_04.webp" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="https://docs.google.com/spreadsheets/d/1mX2H8DzkUzrCt5d3c5jZxgxwJC2fFDwmJsHaHHiJ2g0/edit?usp=sharing" %}
task-04
{% endembed %}

5\. Створіть функцію з двома параметрами, яка буде ділити перший параметр на другий. Обидва параметри та значення, що повертається, повинні мати тип `FLOAT`.

Використайте конструкцію `DROP FUNCTION IF EXISTS`. Застосуйте функцію до атрибута `quantity` таблиці `order_details` . Другим параметром може бути довільне число на ваш розсуд.

```sql
DROP FUNCTION IF EXISTS CalculateDivision;

DELIMITER //
CREATE FUNCTION CalculateDivision(order_quantity FLOAT, divider FLOAT)
RETURNS FLOAT
DETERMINISTIC
BEGIN
	DECLARE result FLOAT;

	IF divider = 0 THEN
		RETURN NULL;
    ELSEIF divider IS NULL THEN
		SET divider = 1.0;
	END IF;
		SET result = order_quantity / divider;

    RETURN result;
END//

DELIMITER ;
```

<figure><img src="../.gitbook/assets/hw-05_task_05.webp" alt="" width="375"><figcaption></figcaption></figure>

Test function:

```sql
SELECT 
    order_id,
    quantity,
    CALCULATEDIVISION(quantity, 2) AS calc_quantity_div
FROM
    order_details
LIMIT 10;
```

{% embed url="https://docs.google.com/spreadsheets/d/1cO1I6t3eVEvE7TsSRaJL7CGi7cW2MTQPGLIPQlg_6bM/edit?usp=sharing" %}
task-05
{% endembed %}

```sql
SELECT 
    order_id,
    quantity,
    CALCULATEDIVISION(quantity, 0) AS calc_quantity_div
FROM
    order_details
LIMIT 10;
```

{% embed url="https://docs.google.com/spreadsheets/d/1rnwZ8Msq74tDfWcizh5EgDe0QadVe0dsAU--vXt-AMY/edit?usp=sharing" %}
task-05
{% endembed %}

```sql
SELECT 
    order_id,
    quantity,
    CALCULATEDIVISION(quantity, NULL) AS calc_quantity_div
FROM
    order_details
LIMIT 10;
```

{% embed url="https://docs.google.com/spreadsheets/d/1cA3XcvOml_8TIQlYMHo_7ejIutptmrmzfF4kpoWBcvU/edit?usp=sharing" %}
task-05
{% endembed %}
