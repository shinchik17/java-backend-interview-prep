#### 40. Строки, соответствующие любому шаблону

**Условие задачи:**  
📌 Есть две таблицы:
- `t1(str)` — строки
- `t2(mask)` — шаблоны для `LIKE`

Необходимо выбрать **все строки** из `t1`, которые **соответствуют хотя бы одному** шаблону из `t2` с помощью `SQL LIKE`.

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Используйте `JOIN` по условию `t1.str LIKE t2.mask`.  
💡 Примените `DISTINCT`, чтобы убрать дубликаты, если одна строка попадает под несколько масок.  
💡 Альтернативно можно использовать `EXISTS` с подзапросом для проверки наличия хотя бы одной маски.  
{{< /details >}}

{{< details "Решение" close >}}
```sql
-- Решение 1: JOIN + DISTINCT
SELECT DISTINCT t1.str
FROM t1
JOIN t2
  ON t1.str LIKE t2.mask;

-- Решение 2: EXISTS
SELECT t1.str
FROM t1
WHERE EXISTS (
    SELECT 1
    FROM t2
    WHERE t1.str LIKE t2.mask
);
````

{{< /details >}}