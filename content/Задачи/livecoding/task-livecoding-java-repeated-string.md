#### 36. Решение задачи HackerRank "Repeat String"

**Условие задачи:**  
🔥 Дан строка `s`, состоящая из строчных английских букв, которая повторяется бесконечно. Дано целое число `n`. Необходимо посчитать количество букв `'a'` в первых `n` символах этой бесконечной строки.

**Пример:**  
- s = "abcac", n = 10  
- Подстрока из первых 10 символов: "abcacabcac".  
- Количество букв 'a': 4


```java

class Result {

    /*
     * Complete the 'repeatedString' function below.
     *
     * The function is expected to return a LONG_INTEGER.
     * The function accepts following parameters:
     *  1. STRING s
     *  2. LONG_INTEGER n
     */
}

```



{{< hint warning >}} **Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}} 
💡 **Используй деление:** Определи число полных повторений строки, разделив `n` на длину строки `s`.  
💡 **Подсчитай 'a':** Сначала посчитай количество `'a'` в строке `s`, а затем умножь на число полных повторений.  
💡 **Обработка остатка:** Не забудь проверить первые `n % s.length()` символов для корректного подсчёта. {{< /details >}}

{{< details "Решение" close >}}

```java
public class Result {

    public static long repeatedString(String s, long n) {
        // Подсчёт количества 'a' в строке s
        long countA = 0;
        for (char c : s.toCharArray()) {
            if (c == 'a') {
                countA++;
            }
        }

        // Определяем, сколько полных повторов строки укладывается в n символов
        long fullRepeats = n / s.length();
        long totalA = fullRepeats * countA;
        
        // Обработка остатка: считаем 'a' в первых (n % s.length()) символах
        int remainingChars = (int) (n % s.length());
        for (int i = 0; i < remainingChars; i++) {
            if (s.charAt(i) == 'a') {
                totalA++;
            }
        }
        return totalA;
    }
    
    public static void main(String[] args) {
        String s = "abcac";
        long n = 10;
        System.out.println("Количество 'a': " + repeatedString(s, n)); // Ожидаемый вывод: 4
    }
}
```

📌 **Что улучшилось?**  
✅ **Оптимальность:** Вместо создания бесконечной строки алгоритм учитывает полные повторения и остаток, что эффективно при больших `n`.  
✅ **Читаемость кода:** Ясное разделение логики на подсчёт `a` в полном повторении и обработку остатка.  
✅ **Стабильность:** Решение работает корректно даже для очень больших значений `n`. {{< /details >}}