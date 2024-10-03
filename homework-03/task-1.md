# Task 1

Напишіть SQL команду, за допомогою якої можна:

* вибрати всі стовпчики (за допомогою wildcard <mark style="color:red;">**\***</mark>) з таблиці `products`;
* вибрати тільки стовпчики _name_, _phone_ з таблиці `shippers`_,_

та перевірте правильність її виконання в MySQL Workbench.



```sql
SELECT * FROM mydb.products;
```

<figure><img src="../.gitbook/assets/task-1.01.webp" alt="" width="375"><figcaption></figcaption></figure>

Файл з результатами у CSV-форматі

{% file src="../.gitbook/assets/task-1.01.csv" %}

<pre class="language-sql"><code class="lang-sql"><strong>SELECT name, phone FROM mydb.shippers;
</strong></code></pre>

<figure><img src="../.gitbook/assets/task-1.02.webp" alt="" width="375"><figcaption></figcaption></figure>

Файл з результатами у CSV-форматі

{% file src="../.gitbook/assets/task-1.02.csv" %}
