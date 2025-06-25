#### 43. Проверка простого числа


**Условие задачи:**  
🔥 Написать консольное приложение на Java, которое определяет, является ли введённое пользователем число простым.

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Число больше 1 — только такие могут быть простыми.  
💡 Проверяйте делимость на 2 отдельно, затем только нечётные делители до √n.  
💡 Если делитель найден — число составное, можно сразу вернуть `false`.  
💡 Не забывайте обрабатывать `n <= 1` как не простые.  
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.Scanner;

public class PrimeChecker {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Введите число: ");
        int number = scanner.nextInt();
        if (isPrime(number)) {
            System.out.println(number + " — простое число.");
        } else {
            System.out.println(number + " — не является простым.");
        }
        scanner.close();
    }

    public static boolean isPrime(int n) {
        if (n <= 1) {
            return false; // 0 и отрицательные не простые
        }
        if (n == 2) {
            return true;  // 2 — единственное чётное простое
        }
        if (n % 2 == 0) {
            return false; // все другие чётные составные
        }
        int limit = (int) Math.sqrt(n);
        for (int i = 3; i <= limit; i += 2) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

📌 **Что важно помнить:**  
✅ Обработка крайних случаев (`n <= 1`, `n == 2`).  
✅ Оптимизация: проверка только нечётных делителей до корня.  
✅ Немедленный выход при первом найденном делителе.  
{{< /details >}}

