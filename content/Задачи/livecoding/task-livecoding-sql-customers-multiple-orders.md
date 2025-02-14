#### 10. Выбрать всех клиентов, у которых больше трех заказов с ценой > 10

```
Customer
- Id
- name

Order
- Id
- customer_id
- Price

```



{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔍 Мы должны выбрать всех клиентов, которые сделали **более трех заказов**, где **цена** каждого заказа больше **10**.  
📊 Для этого нужно группировать заказы по **`customer_id`** и подсчитывать количество заказов с ценой выше 10.  
📌 Используем **`HAVING`** для фильтрации по количеству заказов.
{{< /details >}}

{{< details "Решение" close >}}

```sql
SELECT o.customer_id, c.name
FROM Order o
JOIN Customer c ON o.customer_id = c.Id
WHERE o.Price > 10
GROUP BY o.customer_id, c.name
HAVING COUNT(o.Id) > 3;
```

📌 **Объяснение:**

- **`JOIN`** соединяет таблицы **`Order`** и **`Customer`** по полю **`customer_id`**.
- **`WHERE o.Price > 10`** фильтрует только те заказы, где цена больше 10.
- **`GROUP BY o.customer_id, c.name`** группирует по **`customer_id`** и имени клиента.
- **`HAVING COUNT(o.Id) > 3`** выбирает только тех клиентов, у которых больше 3 заказов с ценой больше 10.

✅ **Задача решена!** 🎯
{{< /details >}}
