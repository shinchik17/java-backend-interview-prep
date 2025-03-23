#### 14. Поиск пользователей, у которых больше одной машины

**Условие задачи:**  
🚗 **Вывести имена пользователей, у которых более одного автомобиля.**

```sql
-- Создаем таблицу USER
CREATE TABLE USER (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

-- Вставляем данные в таблицу USER
INSERT INTO USER (id, name) VALUES
(1, 'Ivan'),
(2, 'Oleg'),
(3, 'Anna'),
(4, 'Ivan'),
(5, 'Ted');

-- Создаем таблицу CAR
CREATE TABLE CAR (
    id INT PRIMARY KEY,
    model VARCHAR(50) NOT NULL,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES USER(id)
);

-- Вставляем данные в таблицу CAR
INSERT INTO CAR (id, model, user_id) VALUES
(4422, 'Opel 1', 1),
(4523, 'BMV 5', 2),
(4612, 'VW', 3),
(4853, 'BMV 6', 4);
```



{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
💡 Нам нужно сгруппировать данные по `user_id`.  
💡 Используем `HAVING` для фильтрации пользователей, у которых количество автомобилей (`COUNT(user_id)`) больше 1.  
💡 Присоединяем таблицу `USER`, чтобы получить имена.
{{< /details >}}

{{< details "Решение" close >}}

```sql
SELECT u.name 
FROM USER u
JOIN CAR c ON u.id = c.user_id
GROUP BY u.id, u.name
HAVING COUNT(c.id) > 1;
```

✅ **Объяснение решения:**

- Используем `JOIN`, чтобы соединить пользователей и их автомобили.
- Группируем результат по `user_id`, так как нам нужно считать количество машин у каждого пользователя.
- `HAVING COUNT(c.id) > 1` фильтрует только тех, у кого машин больше одной.

🔥 Теперь запрос вернет список пользователей, у которых больше одного автомобиля! 🚀
{{< /details >}}