#### 14. Написать реализацию метода findPersonByName(). Из списка persons найти человека с именем name

```java
class Person {
    String name;
    Integer age;
}

Optional<Person> findPersonByName(List<Person> persons, String name) {
//...
}

```


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔹 Нам нужно найти объект `Person`, у которого `name` совпадает с переданным.  
🔹 Лучше вернуть `Optional<Person>`, чтобы избежать `null`.  
🔹 Можно использовать `stream()` и `filter()`, либо традиционный `for`-цикл.
{{< /details >}}

{{< details "Решение" close >}}
  
✅ **С использованием `stream()` (современный вариант)**

```java
import java.util.List;
import java.util.Optional;

public class PersonFinder {
    public static Optional<Person> findPersonByName(List<Person> persons, String name) {
        return persons.stream()
                .filter(person -> person.name.equals(name))
                .findFirst();
    }
}
```

📌 **Объяснение:**

- `stream()` создает поток элементов списка.
- `filter()` пропускает только тех, у кого `name` совпадает.
- `findFirst()` берет первый найденный элемент.
- Используем `Optional<Person>`, чтобы избежать `null`.

✅ **С использованием `for`-цикла (традиционный вариант)**

```java
public static Optional<Person> findPersonByName(List<Person> persons, String name) {
    for (Person person : persons) {
        if (person.name.equals(name)) {
            return Optional.of(person);
        }
    }
    return Optional.empty();
}
```

📌 **Объяснение:**

- Пробегаем список `persons`.
- Если находим совпадение, оборачиваем результат в `Optional.of()`.
- Если ничего не найдено – возвращаем `Optional.empty()`.

🔥 **Какой вариант лучше?**  
✅ `stream()` – элегантный и читаемый, но может быть немного медленнее из-за создания потока.  
✅ `for`-цикл – быстрее для небольших списков и понятен новичкам.

Выбор зависит от контекста 🚀
{{< /details >}}
