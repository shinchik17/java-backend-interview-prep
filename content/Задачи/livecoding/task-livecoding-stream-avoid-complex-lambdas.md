#### 2. Упростить вложенные лямбда-выражения

**Условие задачи:**  
📌 Разобраться, что делает данный код, и улучшить читаемость за счёт рефакторинга.

```java
public class AvoidComplexLambdas {
    private final Set<User> users = new HashSet<>();

    public Set<User> findEditors() {
        return users.stream()
                .filter(u -> u.getRoles().stream()
                        .anyMatch(r -> r.getPermissions().contains(Permission.EDIT)))
                .collect(toSet());
    }
}
```

---

{{< hint warning >}}  
**Спойлеры к ответу**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Метод `findEditors()` ищет пользователей, которые имеют хотя бы одну роль с разрешением `EDIT`.  
💡 Вложенные лямбды ухудшают читаемость.  
💡 Хорошей практикой будет **вынести вложенную логику** в отдельный метод с понятным названием.  
{{< /details >}}

{{< details "Решение" close >}}

### 🔍 Что делает код?

Метод `findEditors()`:

- Проходит по всем `users`,
    
- Фильтрует тех, у кого хотя бы одна `Role` содержит разрешение `Permission.EDIT`,
    
- Возвращает подходящих пользователей как `Set`.
    

### ✅ Как улучшить?

Выносим вложенные лямбды в отдельный метод `hasEditPermission(User u)`:

```java
public class AvoidComplexLambdas {
    private final Set<User> users = new HashSet<>();

    public Set<User> findEditors() {
        return users.stream()
                .filter(this::hasEditPermission)
                .collect(Collectors.toSet());
    }

    private boolean hasEditPermission(User user) {
        return user.getRoles().stream()
                .anyMatch(role -> role.getPermissions().contains(Permission.EDIT));
    }
}
```

### ✨ Преимущества:

- Читаемость повышается.
    
- Появляется возможность переиспользовать метод `hasEditPermission`.
    
- Упрощается отладка и тестирование.
    

🔥 Рефакторинг — это про чистый код, а не только про короткий код! 🧹  
{{< /details >}}