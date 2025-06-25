#### 41. Пользователи с более чем одной покупкой в день

**Условие задачи:**  
📌 Есть таблица `T1(date, user_id)` с датами покупок пользователей:
```

date | user_id  
------------+---------  
2017-01-01 | user1  
2017-01-01 | user1  
2017-01-02 | user1  
2017-01-01 | user2  
2017-01-02 | user2  
2017-01-03 | user3

````
Нужно найти все случаи, когда пользователь совершил более одной покупки в один и тот же день, и вывести пары `(date, user_id)`.

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Первый вариант: сгруппировать по `(date, user_id)` и отфильтровать `HAVING COUNT(*) > 1`.  
💡 Второй вариант (без `GROUP BY`): использовать оконную функцию `COUNT(*) OVER (PARTITION BY date, user_id)`.  
{{< /details >}}

{{< details "Решение" close >}}
```sql
-- Вариант 1: GROUP BY + HAVING
SELECT
    date,
    user_id
FROM T1
GROUP BY
    date,
    user_id
HAVING COUNT(*) > 1;

-- Вариант 2: Оконная функция (без GROUP BY)
SELECT DISTINCT
    date,
    user_id
FROM (
    SELECT
        date,
        user_id,
        COUNT(*) OVER (PARTITION BY date, user_id) AS purchase_count
    FROM T1
) sub
WHERE purchase_count > 1;
````

{{< /details >}}

