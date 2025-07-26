#### 56. Self‑inject для корректного создания транзакций

**Условие задачи:**  
Дан код. Есть сервис с двумя методами a() и b(). Из b() вызывается a(). Если извне происходит вызов метода b(), сколько будет создано транзакций? Как поправить тот факт, что в таком вызове будет 0 транзакций, чтобы proxy создался и отработала вся логика?  Напиши self-inject


**Код:**

```java
@Service
class SomeService {
    @Transactional
    public void a() {}

    public void b() {
        a();
    }
}

SomeService.b();
````

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Spring AOP создает прокси вокруг бина и перехватывает только **внешние** вызовы.  
💡 Внутренний вызов через `this.a()` **обходит** прокси, поэтому аннотация `@Transactional` не применяется.  
💡 Нужно вызывать `a()` через прокси-бин: внедрить сам `SomeService` в него же (self‑injection).  
{{< /details >}}

{{< details "Решение" close >}}

```java
@Service
public class SomeService {
    // self‑inject прокси
    private final SomeService self;

    public SomeService(SomeService self) {
        this.self = self;
    }

    @Transactional
    public void a() {
        // логика, работающая в транзакции
    }

    public void b() {
        // вместо this.a() использовать прокси
        self.a();
    }
}
```

**Что происходит:**

- При вызове `someService.b()` внешне – транзакция на `b()` не нужна.

- Внутри `b()` вызов `self.a()` проходит через Spring‑прокси, что позволяет `@Transactional` на `a()` создать новую транзакцию.


Таким образом, **одна транзакция** будет создана для метода `a()`.  
{{< /details >}}