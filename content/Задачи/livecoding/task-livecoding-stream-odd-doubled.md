#### 6. Stream API — удвоенные нечётные числа

**Условие задачи:**  
🔥 Дан массив натуральных чисел от 0 до 100. С помощью **Stream API** нужно получить коллекцию удвоенных нечетных чисел.  

Пример:  
Вход: `[0, 1, 2, 3, 4, 5]`  
Выход: `[2, 6, 10]`

{{< hint warning >}}
**Спойлеры к решению**
{{< /hint >}}

{{< details "Подсказки" close >}}
💡 Используй `IntStream.rangeClosed(0, 100)` для генерации чисел.  
💡 Чтобы оставить только нечетные — `.filter(n -> n % 2 != 0)`.  
💡 Чтобы удвоить — `.map(n -> n * 2)`.  
💡 Чтобы собрать в коллекцию — `.boxed().collect(Collectors.toList())`.  
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class TaskStreamOddDoubled {
    public static void main(String[] args) {
        List<Integer> result = IntStream.rangeClosed(0, 100)
                .filter(n -> n % 2 != 0)
                .map(n -> n * 2)
                .boxed()
                .collect(Collectors.toList());

        System.out.println(result);
    }
}
```

Мы генерируем числа от 0 до 100 через `IntStream.rangeClosed`.  
Отбираем нечетные фильтром.  
Применяем `map` для удвоения каждого числа.  
Дальше преобразуем к объектам (`.boxed()`) и собираем в список. ✅  

{{< /details >}}