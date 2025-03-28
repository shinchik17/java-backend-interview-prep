#### 13. Пользователь и его машины: связь в базе данных

**Условие задачи:**  
🔍 **Создать структуру базы данных, где один пользователь может владеть несколькими машинами, а каждая машина принадлежит только одному пользователю.**


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
💡 В `CAR` должна быть внешняя связь (`FOREIGN KEY`) с `USER`, указывающая на владельца.  
💡 Каждый `USER` может иметь несколько записей в `CAR`, но каждая `CAR` должна быть привязана к одному `USER`.  
💡 Можно использовать `JOIN`, чтобы получить информацию о пользователях и их автомобилях.
{{< /details >}}

{{< details "Решение" close >}}

```sql
-- Создаем таблицу пользователей
CREATE TABLE USER (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

-- Заполняем таблицу пользователей
INSERT INTO USER (id, name) VALUES
(1, 'Ivan'),
(2, 'Oleg'),
(3, 'Anna'),
(4, 'Ivan'),
(5, 'Ted');

-- Создаем таблицу автомобилей
CREATE TABLE CAR (
    id INT PRIMARY KEY,
    model VARCHAR(50) NOT NULL,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES USER(id) ON DELETE CASCADE
);

-- Заполняем таблицу автомобилей
INSERT INTO CAR (id, model, user_id) VALUES
(4422, 'Opel 1', 1),
(4523, 'BMW 5', 2),
(4612, 'VW', 3),
(4853, 'BMW 6', 4);

-- Запрос для получения списка пользователей и их машин
SELECT 
    u.id AS user_id, 
    u.name AS user_name, 
    c.id AS car_id, 
    c.model AS car_model
FROM USER u
LEFT JOIN CAR c ON u.id = c.user_id
ORDER BY u.id;
```

✅ **Объяснение решения:**

- Создаем таблицу `USER`, где `id` — первичный ключ.
- Создаем таблицу `CAR`, где `user_id` — внешний ключ, связывающий машину с пользователем.
- Добавляем `ON DELETE CASCADE`, чтобы при удалении пользователя удалялись и его машины.
- `LEFT JOIN` позволяет вывести всех пользователей, даже если у них нет машины.
- `ORDER BY u.id` упорядочивает результат по пользователям.

🚀 **Теперь можно легко получать информацию о пользователях и их машинах!** 🔥
{{< /details >}}