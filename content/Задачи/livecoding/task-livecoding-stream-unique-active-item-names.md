#### 1. Создать уникальную коллекцию (типа String) активных (атрибут Item.active со значением true) имен (атрибут Item.name), используя в качестве входных данных список items

```java
@Getter
@Setter
class Item {
    private final Long id;
    private String name;
    private boolean active;
}

List<Item> items = ...


Collection<String> result = items.stream()...

```


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
💡 Мы можем использовать **Stream API** в Java для фильтрации элементов из списка и сбора результатов в уникальную коллекцию.  
🔍 Для этого воспользуемся **`filter`** для выбора активных элементов и **`map`** для извлечения имен. Затем используем **`Collectors.toSet()`** для того, чтобы результат был уникальным.
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.*;
import java.util.stream.*;

@Getter
@Setter
class Item {
    private final Long id;
    private String name;
    private boolean active;
}

public class Main {
    public static void main(String[] args) {
        List<Item> items = ...; // Ваш список объектов Item

        Collection<String> result = items.stream()
            .filter(Item::isActive)               // Фильтруем активные элементы
            .map(Item::getName)                   // Извлекаем имя
            .collect(Collectors.toSet());         // Собираем в уникальную коллекцию

        System.out.println(result);
    }
}
```

📌 **Объяснение:**

- **`filter(Item::isActive)`** — фильтруем элементы, где **`active == true`**.
- **`map(Item::getName)`** — извлекаем имена активных элементов.
- **`collect(Collectors.toSet())`** — собираем результат в **`Set`**, который автоматически исключает дубликаты, обеспечивая уникальность.

🚀 **Результат:**

- Мы получим коллекцию уникальных имен активных предметов.
{{< /details >}}