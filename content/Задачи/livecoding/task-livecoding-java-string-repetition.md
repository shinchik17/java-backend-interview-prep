#### 35. Проверка повторяющихся символов в строке

**Условие задачи:**  
🔥 Реализуйте метод, который принимает на вход непустую строку и проверяет, что в строке отсутствуют символы, повторяющиеся более двух раз. Метод возвращает **true**, если в строке нет символов с более чем двумя повторениями, и **false**, если такие символы присутствуют.


{{< hint warning >}} **Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}} 
💡 **Подсчет повторений:** Используйте `Map<Character, Integer>` для подсчета количества появлений каждого символа в строке.  
💡 **Проверка лимита:** Если для любого символа количество появлений становится больше двух, сразу возвращайте `false`.  
💡 **Обработка входных данных:** Добавьте проверку на пустую строку или `null` для повышения надежности. {{< /details >}}

{{< details "Решение" close >}}

```java
public class StringUtils {
    public static boolean validateString(String input) {
        if (input == null || input.isEmpty()) {
            throw new IllegalArgumentException("Строка не должна быть пустой");
        }
        
        // Хранение количества повторений каждого символа
        Map<Character, Integer> charCount = new HashMap<>();
        for (char c : input.toCharArray()) {
            int count = charCount.getOrDefault(c, 0) + 1;
            if (count > 2) {
                return false;
            }
            charCount.put(c, count);
        }
        return true;
    }
    
    public static void main(String[] args) {
        String validExample = "aabbcc"; // все символы повторяются не более двух раз
        String invalidExample = "aaabbc"; // символ 'a' повторяется 3 раза
        
        System.out.println("Пример 1: " + validExample + " -> " + validateString(validExample)); // true
        System.out.println("Пример 2: " + invalidExample + " -> " + validateString(invalidExample)); // false
    }
}
```

📌 **Что улучшилось?**  
✅ **Эффективный подсчет:** Применение карты для быстрого подсчета повторений символов.  
✅ **Ранняя остановка:** Метод возвращает `false`, сразу как только встречается символ, повторяющийся более двух раз.  
✅ **Обработка ошибок:** Добавлена проверка на `null` и пустую строку для повышения надежности метода. {{< /details >}}