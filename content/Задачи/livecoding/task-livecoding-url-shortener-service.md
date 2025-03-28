#### 7. Спроектировать и реализовать сервис для сокращения URL-адресов?


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
- Для реализации сервиса сокращения URL можно использовать **хеширование** или создание уникальных идентификаторов.
- Можно использовать структуру данных, которая будет хранить пары **исходный URL** → **сокращенный URL**.
- Каждый раз, когда пользователь вводит длинный URL, мы генерируем уникальный идентификатор для этого URL и сохраняем его в базе данных или памяти.
- Для хранения данных можно использовать **Map**, где ключом будет уникальный идентификатор, а значением — оригинальный URL.
- После этого, при запросе по сокращенному URL, сервис должен возвращать оригинальный URL.
{{< /details >}}

{{< details "Решение" close >}}

Для начала реализуем простой сервис сокращения URL-адресов с использованием Map для хранения данных и генерации коротких URL.

 **Шаг 1. Создадим интерфейс сервиса**

```java
public interface URLShorteningService {
    String shortenURL(String originalURL);
    String getOriginalURL(String shortenedURL);
}
```

 **Шаг 2. Реализуем сервис**

```java
import java.util.HashMap;
import java.util.Map;

public class URLShorteningServiceImpl implements URLShorteningService {

    private static final String BASE_URL = "http://short.ly/";
    private Map<String, String> urlStorage = new HashMap<>();
    private static final String CHARACTERS = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private static final int URL_LENGTH = 6;  // Длина сокращенного URL

    @Override
    public String shortenURL(String originalURL) {
        String shortURL = generateShortURL();
        urlStorage.put(shortURL, originalURL);
        return BASE_URL + shortURL;
    }

    @Override
    public String getOriginalURL(String shortenedURL) {
        String shortURL = shortenedURL.replace(BASE_URL, "");
        return urlStorage.getOrDefault(shortURL, null);
    }

    // Генерация случайного короткого URL
    private String generateShortURL() {
        StringBuilder shortURL = new StringBuilder();
        for (int i = 0; i < URL_LENGTH; i++) {
            int index = (int) (Math.random() * CHARACTERS.length());
            shortURL.append(CHARACTERS.charAt(index));
        }
        return shortURL.toString();
    }
}
```

 **Шаг 3. Пример использования сервиса**

```java
public class Main {
    public static void main(String[] args) {
        URLShorteningService urlShorteningService = new URLShorteningServiceImpl();

        // Сокращение URL
        String originalURL = "https://www.example.com/long-url";
        String shortenedURL = urlShorteningService.shortenURL(originalURL);
        System.out.println("Shortened URL: " + shortenedURL);

        // Получение оригинального URL
        String retrievedURL = urlShorteningService.getOriginalURL(shortenedURL);
        System.out.println("Original URL: " + retrievedURL);
    }
}
```

** Ключевые моменты:**

1. **Класс `URLShorteningServiceImpl`** реализует логику сокращения URL и возвращения оригинала.
2. **Генерация короткого URL** происходит с использованием случайных символов, из которых формируется строка заданной длины.
3. Для хранения пары **оригинальный URL → сокращенный URL** используется **Map**.
4. Метод **`shortenURL`** генерирует сокращенный URL, сохраняет его в хранилище и возвращает.
5. Метод **`getOriginalURL`** извлекает оригинальный URL из хранилища по сокращенному URL.

Такой сервис будет работать для базового сокращения URL, а также может быть расширен дополнительными функциями, например:

- Проверка на дубликаты сокращенных URL.
- Хранение статистики по кликам.
- Сроки действия сокращенных URL.
{{< /details >}}
