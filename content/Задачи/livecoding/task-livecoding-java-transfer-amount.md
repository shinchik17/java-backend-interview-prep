#### 53. Перевод средств между аккаунтами

Есть Spring сервис передачи денег с одного аккаунта на другой. Есть метод transferAmount(), который реализует эту логику. Этот метод принимает 3 параметра: аккаунт с которого переводим, куда переводим и amount сумму перевода. Задачи: выбрать тип amount, обосновать выбор. Реализовать логику перевода. Фокусируемся на валидации, бизнес-логике, корректности решения, производительности. БД нет, все храним в памяти. Сетевого взаимодействия по REST нет


**Условие задачи:**  
📌 Реализовать метод `transferAmount(Account from, Account to, ??? amount)` в сервисе, который переводит деньги из одного аккаунта в другой.
- Всё хранится в памяти — без БД.
- Без сетевого взаимодействия.
- Фокус на **выборе типа** для суммы, **валидации**, **бизнес‑логике**, **корректности** и **производительности**.

**Код:**

```java
class Account {
    // TODO Сюда можно добавлять поля
}

class TransferService {
    void transferAmount(Account from, Account to, ??? amount) {
        // TODO Сюда добавляем логику списания/пополнения счетов
    }
}
````

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 **Тип `BigDecimal`** — подходит для денег: точные вычисления без потерь.  
💡 Проверьте, что `from`, `to` и `amount` не `null`, и `amount > 0`.  
💡 Убедитесь, что `from` и `to` — разные аккаунты.  
💡 Перед списанием проверяйте, что `from.getBalance().compareTo(amount) >= 0`.  
💡 Для потокобезопасности: блокируйте аккаунты в порядке `id`, чтобы избежать дедлока.  
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.math.BigDecimal;
import java.util.Objects;

public class TransferService {

    public void transferAmount(Account from, Account to, BigDecimal amount) {
        // Валидация входных параметров
        Objects.requireNonNull(from, "Source account must not be null");
        Objects.requireNonNull(to,   "Target account must not be null");
        Objects.requireNonNull(amount, "Amount must not be null");
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        if (from.getId().equals(to.getId())) {
            throw new IllegalArgumentException("Cannot transfer to the same account");
        }

        // Выбираем порядок блокировок по id для избежания дедлоков
        Account first  = from.getId().compareTo(to.getId()) < 0 ? from : to;
        Account second = first == from ? to : from;

        synchronized (first) {
            synchronized (second) {
                // Проверка достаточности средств
                if (from.getBalance().compareTo(amount) < 0) {
                    throw new IllegalArgumentException(
                        "Insufficient funds in account " + from.getId());
                }
                // Выполняем перевод
                from.debit(amount);
                to.credit(amount);
            }
        }
    }
}
```

{{< /details >}}