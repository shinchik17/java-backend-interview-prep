#### 5. Даны две таблицы Persosn и Payments. Один-ко-многим. Справа описание. Напишите запрос, который выводит name и value

```
Даны две таблицы :
Persons co списком работников

id name
1 Petya
2 Vasya
3 Kolya

Payments с зарплатными начислениями ежемесячно

id persons_id value
1 1 10
2 1 20
3 3 15

```



{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
📝 Нужно **объединить две таблицы** (`Persons` и `Payments`) по `persons_id`.  
🔗 Используем **`JOIN`** для связи.  
📊 Выводим **имя (`name`) и сумму `value` для каждого платежа**.
{{< /details >}}

{{< details "Решение" close >}}

```sql
SELECT p.name, pay.value
FROM Persons p
JOIN Payments pay ON p.id = pay.persons_id;
```

📌 **Объяснение:**

- **`JOIN Persons p ON p.id = pay.persons_id`** связывает таблицы по `id`.
- **Выбираем `p.name` и `pay.value`** – имя работника и его зарплату.

🔥 **Вывод результата**:

|name|value|
|---|---|
|Petya|10|
|Petya|20|
|Kolya|15|

✅ **Простой и эффективный запрос** 🚀
{{< /details >}}