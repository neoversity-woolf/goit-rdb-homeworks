---
icon: server
---

# Final Project

1\. Завантажте дані:

* Створіть схему `pandemic` у базі даних за допомогою SQL-команди.
* Оберіть її як схему за замовчуванням за допомогою SQL-команди.
* Імпортуйте [дані](https://drive.google.com/file/d/1lHEXJvu2omYRgvSek6mHq-iQ3RmGAQ7e/view?usp=sharing) за допомогою Import wizard так, як ви вже робили це у темі 3.
* Продивіться дані, щоб бути у контексті.

```sql
-- 1
CREATE SCHEMA IF NOT EXISTS pandemic;

USE pandemic;

-- Імпортуємо дані з CSV файла за допомогою Table Data Import Wizard

-- Досліджуємо дані таблиці
SELECT * FROM infectious_cases_old LIMIT 10;
SELECT COUNT(*) AS records FROM infectious_cases_old; -- 10521
```

{% embed url="https://docs.google.com/spreadsheets/d/1IktMWSy0vTGXPGfT6c7SCQ49BCiMcjFJMVCKjgx9WLg/edit?usp=sharing" %}
query result
{% endembed %}

<div>

<figure><img src=".gitbook/assets/fp_task-01-1.webp" alt="" width="188"><figcaption></figcaption></figure>

 

<figure><img src=".gitbook/assets/fp_task-01-2.webp" alt="" width="188"><figcaption></figcaption></figure>

 

<figure><img src=".gitbook/assets/fp_task-01-3.webp" alt="" width="188"><figcaption></figcaption></figure>

</div>

Дослідивши вхідні дані бачимо:

* Всього було імпортовано 10521 рядків
* Колонка `Entity` з назвами країн містить 245 унікальних значень
* Колонка `Code` з кодом країн містить 211 унікальних значень

2. Нормалізуйте таблицю infectious\_cases до 3ї нормальної форми. Збережіть у цій же схемі дві таблиці з нормалізованими даними.

```sql
-- 2 Приводимо таблицю infectious_cases_old до 3NF
CREATE TABLE IF NOT EXISTS entities (
      entity_id INT PRIMARY KEY AUTO_INCREMENT
    , entity VARCHAR(45)
    , code VARCHAR(10)
);

INSERT INTO entities (entity, code) SELECT DISTINCT entity, code FROM infectious_cases_old;
SELECT * FROM entities;

CREATE TABLE infectious_cases (
      case_id INT PRIMARY KEY AUTO_INCREMENT
    , entity_id INT
    , year YEAR
    , number_yaws TEXT
    , polio_cases TEXT
    , cases_guinea_worm TEXT
    , number_rabies TEXT
    , number_malaria TEXT
    , number_hiv TEXT
    , number_tuberculosis TEXT
    , number_smallpox TEXT
    , number_cholera_cases TEXT
    , FOREIGN KEY (entity_id) REFERENCES entities(entity_id)
);

INSERT INTO infectious_cases (
      entity_id
    , year
    , number_yaws
    , polio_cases
    , cases_guinea_worm
    , number_rabies
    , number_malaria
    , number_hiv
    , number_tuberculosis
    , number_smallpox
    , number_cholera_cases
)
SELECT 
      e.entity_id
    , ico.year
    , ico.number_yaws
    , ico.polio_cases
    , ico.cases_guinea_worm
    , ico.number_rabies
    , ico.number_malaria
    , ico.number_hiv
    , ico.number_tuberculosis
    , ico.number_smallpox
    , ico.number_cholera_cases
FROM infectious_cases_old ico
JOIN entities e ON ico.entity = e.entity AND ico.code = e.code;

SELECT * FROM infectious_cases LIMIT 10;
```

{% embed url="https://docs.google.com/spreadsheets/d/1mvBN7CJnZBEux9nIZptDSpAbXWlJrTDUSYw0VKWmO0Q/edit?usp=sharing" %}
query result
{% endembed %}

<div>

<figure><img src=".gitbook/assets/fp_task-02-1.webp" alt=""><figcaption></figcaption></figure>

 

<figure><img src=".gitbook/assets/fp_task-02-2.webp" alt=""><figcaption></figcaption></figure>

</div>

3\. Проаналізуйте дані:

* Для кожної унікальної комбінації `Entity` та `Code` або їх `id` порахуйте середнє, мінімальне, максимальне значення та суму для атрибута `Number_rabies`.
* Результат відсортуйте за порахованим середнім значенням у порядку спадання.
* Оберіть тільки 10 рядків для виведення на екран.

<pre class="language-sql"><code class="lang-sql">-- 3
SELECT
      e.entity
    , e.code
<strong>    , AVG(ic.number_rabies) AS avg_rabies
</strong>    , MIN(ic.number_rabies) AS min_rabies
    , MAX(ic.number_rabies) AS max_rabies
    , SUM(ic.number_rabies) AS sum_rabies
FROM infectious_cases ic
JOIN entities e ON ic.entity_id = e.entity_id
WHERE ic.number_rabies NOT IN ('')
GROUP BY e.entity, e.code
LIMIT 10;
</code></pre>



{% embed url="https://docs.google.com/spreadsheets/d/1FUw4os-zpt_UHxYAsh7BN-pcXvrpJwCa-uBWcjSD_-c/edit?usp=sharing" %}
query result
{% endembed %}

<figure><img src=".gitbook/assets/fp_task-03.webp" alt="" width="375"><figcaption></figcaption></figure>

4\. Побудуйте колонку різниці в роках.

Для оригінальної або нормованої таблиці для колонки `Year` побудуйте з використанням вбудованих SQL-функцій:

* атрибут, що створює дату першого січня відповідного року
* атрибут, що дорівнює поточній даті
* атрибут, що дорівнює різниці в роках двох вищезгаданих колонок

```sql
-- 4
SELECT 
      MAKEDATE(year, 1) AS past_date
    , CURDATE() AS curr_date
    , DATEDIFF(CURDATE(), MAKEDATE(year, 1)) AS diff_days
FROM
    infectious_cases
LIMIT 10;
```



{% embed url="https://docs.google.com/spreadsheets/d/1ZFXoI5edNdaYTNP0jMSZ00IrMDmwHHDoQW-QnE8hH_0/edit?usp=sharing" %}
query result
{% endembed %}

<figure><img src=".gitbook/assets/fp_task-04.webp" alt="" width="375"><figcaption></figcaption></figure>

5\. Побудуйте власну функцію.

* Створіть і використайте функцію, що будує такий же атрибут, як і в попередньому завданні: функція має приймати на вхід значення року, а повертати різницю в роках між поточною датою та датою, створеною з атрибута року (1996 рік → '1996-01-01').

```sql
DROP FUNCTION IF EXISTS diff_days;

DELIMITER //

CREATE FUNCTION diff_days(year YEAR)
RETURNS INT
DETERMINISTIC
BEGIN
	DECLARE past_date DATE;
    DECLARE curr_date DATE;
    SET past_date = MAKEDATE(year, 1);
	SET curr_date = CURDATE();
    RETURN DATEDIFF(curr_date, past_date);
END//

DELIMITER ;

SELECT 
      MAKEDATE(year, 1) AS past_date
    , CURDATE() AS curr_date
    , diff_days(year) AS diff_days
FROM
    infectious_cases
LIMIT 10;
```



{% embed url="https://docs.google.com/spreadsheets/d/1N5X2UEstKopyzUEQ4bXvAejc-EpYOKMI26myOjY67oE/edit?usp=sharing" %}
query result
{% endembed %}

<figure><img src=".gitbook/assets/fp_task-05.webp" alt="" width="375"><figcaption></figcaption></figure>
