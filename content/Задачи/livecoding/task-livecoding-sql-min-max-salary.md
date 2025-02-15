#### 6. Необходимо написать sql запрос, который вернет минимальную и максимальную зарплату по всем отделам среди не уволенных сотрудников

```
В базе данных есть две таблицы: ""units"" - подразделения компании и ""employees"" - сотрудники компании
units:
id int // Primary Key
name text // (название unit)
employees:
id int // Primary Key
unit_id int // Foreign Key -> units::id
salary numeric // (зарплата)
fired boolean // (флаг уволен)

```

{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔗 Нужно **объединить** таблицы `units` и `employees` по `unit_id`.  
⚡ Отфильтровать **не уволенных** сотрудников (`fired = FALSE`).  
📊 Использовать **`GROUP BY`**, чтобы агрегировать зарплаты по отделам.  
📈 **`MIN()` и `MAX()`** помогут найти минимальную и максимальную зарплату.
{{< /details >}}

{{< details "Решение" close >}}

```sql
SELECT 
    u.id AS unit_id,
    u.name AS unit_name,
    MIN(e.salary) AS min_salary,
    MAX(e.salary) AS max_salary
FROM units u
JOIN employees e ON u.id = e.unit_id
WHERE e.fired = FALSE
GROUP BY u.id, u.name;
```

📌 **Объяснение:**

- **`JOIN employees e ON u.id = e.unit_id`** – связываем сотрудников с их отделами.
- **`WHERE e.fired = FALSE`** – исключаем уволенных сотрудников.
- **`MIN(e.salary)` и `MAX(e.salary)`** – вычисляем минимальную и максимальную зарплаты.
- **`GROUP BY u.id, u.name`** – группируем данные по отделам.

🔥 **Вывод примера:**

|unit_id|unit_name|min_salary|max_salary|
|---|---|---|---|
|1|IT|50000|150000|
|2|HR|40000|90000|

✅ **Готово!** 🚀
{{< /details >}}
