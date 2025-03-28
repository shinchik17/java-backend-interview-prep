#### 12. Спроектировать http запрос который возвращает договор по номеру


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔹 Нужно создать **REST-контроллер** с эндпоинтом `GET /contract/{number}`.  
🔹 Договор должен извлекаться из **сервиса** или **репозитория**.  
🔹 Используем `@RestController` и `@GetMapping`.
{{< /details >}}

{{< details "Решение" close >}}
Пример простого Spring Boot REST API:

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/contract")
public class ContractController {
    private final ContractService contractService;

    public ContractController(ContractService contractService) {
        this.contractService = contractService;
    }

    @GetMapping("/{number}")
    public Contract getContract(@PathVariable String number) {
        return contractService.findByNumber(number);
    }
}
```

📌 **Сервис для поиска договора:**

```java
import org.springframework.stereotype.Service;
import java.util.Map;

@Service
public class ContractService {
    private final Map<String, Contract> contractStorage = Map.of(
        "123", new Contract("123", "John Doe"),
        "456", new Contract("456", "Jane Doe")
    );

    public Contract findByNumber(String number) {
        return contractStorage.getOrDefault(number, new Contract("N/A", "Not Found"));
    }
}
```

📌 **Модель данных:**

```java
public class Contract {
    private String number;
    private String client;

    public Contract(String number, String client) {
        this.number = number;
        this.client = client;
    }

    public String getNumber() { return number; }
    public String getClient() { return client; }
}
```

📌 **Пример запроса:**

```
GET /contract/123  
```

📌 **Ответ:**

```json
{
  "number": "123",
  "client": "John Doe"
}
```

🚀 **Вывод:** Мы создали **контроллер**, **сервис** и **модель**, реализовав простой REST API для поиска договора по номеру.
{{< /details >}}