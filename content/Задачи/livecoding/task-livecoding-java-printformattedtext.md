#### 18. Форматирование текста в консоли

**Условие задачи:**  
Напиши метод, который принимает строку и выводит на консоль отформатированный текст в виде рамки. В рамках должна быть отображена каждая часть строки в отдельной строке так, как показано в примере ниже.  
😊 **Пример вывода:**

```
****************
*  I           *
*  am          *
*  Java        *
*  Developer   *
****************
```

---

**Код:**

```java
public class Task1 {
    private static final String TEXT = "I am Java Developer";

    /*
    ****************
    *  I           *
    *  am          *
    *  Java        *
    *  Developer   *
    ****************
    */

    /**
     * Реализовать функцию вывода на консоль текста в параметре TEXT в формате, указанном выше.
     */
    public void printFormattedText(String text) {
        throw new UnsupportedOperationException("Implemented it!");
    }
}
```

---

{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}

- **Разбей строку на слова:**  
    Используй метод `split(" ")`, чтобы получить массив слов.
- **Вычисли максимальную длину слова:**  
    Это поможет правильно установить ширину рамки.
- **Построй рамку:**  
    Используй символ `*` для создания верхней и нижней границы.
- **Форматированный вывод:**  
    При выводе каждого слова используй форматирование, чтобы добавить пробелы до нужной длины.

{{< /details >}}

{{< details "Решение" close >}}

```java
public class Task1 {
    private static final String TEXT = "I am Java Developer";

    /**
     * Функция выводит текст, разбитый на слова, в виде рамки.
     * Пример:
     * ****************
     * *  I           *
     * *  am          *
     * *  Java        *
     * *  Developer   *
     * ****************
     */
    public void printFormattedText(String text) {
        if (text == null || text.trim().isEmpty()) {
            System.out.println("Текст пустой!");
            return;
        }
        
        // Разбиваем строку на слова
        String[] words = text.split("\\s+");
        
        // Находим максимальную длину слова
        int maxLength = 0;
        for (String word : words) {
            if (word.length() > maxLength) {
                maxLength = word.length();
            }
        }
        
        // Формируем верхнюю и нижнюю границы рамки
        String border = "*".repeat(maxLength + 6);
        
        // Вывод рамки
        System.out.println(border);
        for (String word : words) {
            // Форматируем вывод так, чтобы слово выводилось с отступами
            System.out.printf("*  %-"+maxLength+"s  *%n", word);
        }
        System.out.println(border);
    }
    
    public static void main(String[] args) {
        Task1 task = new Task1();
        task.printFormattedText(TEXT);
    }
}
```

{{< /details >}}




