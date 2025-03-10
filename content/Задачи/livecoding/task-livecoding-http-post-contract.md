#### 13. Спроектировать http запрос который создает новый договор


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
🔹 Нужно создать **POST-запрос** `/contract`, который принимает JSON с данными договора.  
🔹 Используем `@PostMapping` и `@RequestBody`.  
🔹 Данные должны сохраняться в **хранилище** (можно использовать `Map` как временную БД).

{{< /details >}}

{{< details "Решение" close >}}
Пример REST API для создания договора:

```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/contract")
public class ContractController {
    private final ContractService contractService;

    public ContractController(ContractService contractService) {
        this.contractService = contractService;
    }

    @PostMapping
    public ResponseEntity<Contract> createContract(@RequestBody Contract contract) {
        Contract savedContract = contractService.save(contract);
        return ResponseEntity.ok(savedContract);
    }
}
```

📌 **Сервис для сохранения договора:**

```java
import org.springframework.stereotype.Service;
import java.util.*;

@Service
public class ContractService {
    private final Map<String, Contract> contractStorage = new HashMap<>();

    public Contract save(Contract contract) {
        contractStorage.put(contract.getNumber(), contract);
        return contract;
    }
}
```

📌 **Модель данных:**

```java
public class Contract {
    private String number;
    private String client;

    public Contract() {} // Для JSON сериализации

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
POST /contract  
Content-Type: application/json
```

📌 **Тело запроса:**

```json
{
  "number": "789",
  "client": "Alice Smith"
}
```

📌 **Ответ:**

```json
{
  "number": "789",
  "client": "Alice Smith"
}
```

🚀 **Вывод:** Мы создали **POST-запрос**, который принимает JSON, сохраняет данные в хранилище и возвращает сохраненный объект.
{{< /details >}}
