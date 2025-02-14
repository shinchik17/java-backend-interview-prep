#### 15. Создать интерфейс с 2 методами. Написать класс, реализующий интерфейс для Integer. В класс можем бесконечно отправлять int-ы. И в любой момент можем вызвать getTopFive() и получить 5 максимальных чисел из тех, что отправили. Можно гуглить. Надо будет запустить, проверить работу

```java

interface I1{
    <T> void putValue(T t);
    List<T> getTopFive();
}
```

{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔹 Нам нужно хранить числа и уметь извлекать 5 максимальных.  
🔹 Подойдет `PriorityQueue`, так как это минимальная куча (min-heap).  
🔹 Размер очереди ограничим 5 элементами, удаляя меньшие значения.  
🔹 Используем `List<Integer>` для хранения результатов.
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.*;

interface I1 {
    <T> void putValue(T t);
    List<Integer> getTopFive();
}

class IntegerTopFive implements I1 {
    private final PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    @Override
    public <T> void putValue(T t) {
        if (t instanceof Integer) {
            int value = (Integer) t;
            minHeap.offer(value);
            if (minHeap.size() > 5) {
                minHeap.poll(); // Удаляем минимальный элемент, если больше 5
            }
        } else {
            throw new IllegalArgumentException("Only integers are allowed");
        }
    }
    
    @Override
    public List<Integer> getTopFive() {
        List<Integer> result = new ArrayList<>(minHeap);
        result.sort(Collections.reverseOrder()); // Сортируем по убыванию
        return result;
    }

    public static void main(String[] args) {
        IntegerTopFive topFive = new IntegerTopFive();
        
        topFive.putValue(10);
        topFive.putValue(5);
        topFive.putValue(20);
        topFive.putValue(3);
        topFive.putValue(8);
        topFive.putValue(25);
        topFive.putValue(15);
        
        System.out.println(topFive.getTopFive()); // [25, 20, 15, 10, 8]
    }
}
```

📌 **Объяснение:**

- **`PriorityQueue<Integer>`** хранит элементы в отсортированном порядке (минимальный в начале).
- **Метод `putValue(T t)`** принимает `Integer`, добавляет в очередь, а если размер > 5, удаляет минимальный элемент.
- **Метод `getTopFive()`** копирует элементы, сортирует по убыванию и возвращает.
- **Пример вызова** показывает, как корректно хранятся 5 максимальных чисел.

🔥 **Преимущества этого решения:**  
✅ Работает за **O(log 5) ≈ O(1)** для вставки и **O(5 log 5) ≈ O(5)** для сортировки.  
✅ Поддерживает **динамическое обновление** максимальных 5 чисел.

🚀 Можно расширить для других типов, но в данном задании требуется `Integer`.
{{< /details >}}