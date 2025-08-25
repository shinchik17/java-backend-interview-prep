#### 5. Палиндром с помощью Stream API


**Условие задачи:**  
Написать метод, который проверяет, является ли строка палиндромом.  
Условие: использовать **Stream API**, решение в один стрим.  
Строка может содержать буквы (разного регистра), цифры, знаки препинания. Нужно учитывать только буквы и цифры.  


Примеры:
- `"A man, a plan, a canal: Panama"` → `true`
- `"I love work in IT"` → `false`

{{< hint warning >}}
**Спойлеры к решению**
{{< /hint >}}

{{< details "Подсказки" close >}}
💡 Сначала очистить строку от всего, кроме букв и цифр.  
💡 Привести её к нижнему регистру.  
💡 Использовать `IntStream.range(0, n/2)` для проверки символов.  
💡 `allMatch` помогает убедиться, что все пары совпали.  
{{< /details >}}

{{< details "Решение" close >}}
Мы приводим строку к нормальной форме: убираем ненужные символы и делаем строчные буквы.  
Затем запускаем один стрим `IntStream.range(0, len/2)` и проверяем симметричные пары символов.  
Если все совпадают — строка палиндром. ✅


```java
package app;

import java.util.stream.IntStream;

public class TaskPalindromeStream {

    public static void main(String[] args) {
        System.out.println(isPalindrome("A man, a plan, a canal: Panama")); // true
        System.out.println(isPalindrome("I love work in IT")); // false
    }

    public static boolean isPalindrome(String s) {
        String filtered = s.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
        return IntStream.range(0, filtered.length() / 2)
                .allMatch(i -> filtered.charAt(i) == filtered.charAt(filtered.length() - 1 - i));
    }
}
```

{{< /details >}}