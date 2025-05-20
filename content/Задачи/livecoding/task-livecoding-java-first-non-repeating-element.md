#### 38. Поиск первого неповторяющегося элемента в массиве

**Условие задачи:**  
🔥 Дан массив целых чисел `{9, 4, 9, 6, 6, 4, 5}`. Необходимо найти первый элемент, который в этом массиве встречается ровно один раз.

**Код:**

```java
public class Main {
    public static void main(String[] args) {
        int[] array = {9, 4, 9, 6, 6, 4, 5};
        // TODO
    }
}
````

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Используйте `LinkedHashMap`, чтобы сохранить порядок вставки и подсчитать количество вхождений каждого числа.   
💡 Первый проход по массиву: для каждого элемента увеличивайте счётчик в карте.   
💡 Второй проход: найдите в исходном массиве первый элемент с частотой `1` в карте.   
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        int[] array = {9, 4, 9, 6, 6, 4, 5};

        // Подсчёт вхождений с сохранением порядка
        Map<Integer, Integer> countMap = new LinkedHashMap<>();
        for (int num : array) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        // Поиск первого уникального элемента
        int firstUnique = -1;
        for (int num : array) {
            if (countMap.get(num) == 1) {
                firstUnique = num;
                break;
            }
        }

        if (firstUnique != -1) {
            System.out.println("Первый неповторяющийся элемент: " + firstUnique);
        } else {
            System.out.println("Уникальных элементов не найдено");
        }
    }
}
```

{{< /details >}}