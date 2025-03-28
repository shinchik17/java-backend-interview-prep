#### 5. Реализовать код для отображения "Тик" и "Так" с разницей в одну секунду, используя два потока, с синхронизацией потоков в Java?


{{< hint warning >}}
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}
- Для синхронизации потоков можно использовать **wait()** и **notify()** или **notifyAll()**.
- Один поток должен печатать "Тик", а другой — "Так".
- Потоки должны работать с разницей в одну секунду.
- Используйте общий объект или блок синхронизации для координации потоков.
{{< /details >}}

{{< details "Решение" close >}}

Вот пример кода для реализации задачи:

```java
public class TickTock {
    private final Object lock = new Object();
    private boolean tickTurn = true;  // Флаг для управления порядком

    public void tick() {
        synchronized (lock) {
            while (!tickTurn) {  // Если очередь не для "Тика", ждем
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println("Тик");
            tickTurn = false;  // Смена очередности
            lock.notify();  // Будим другой поток
        }
    }

    public void tock() {
        synchronized (lock) {
            while (tickTurn) {  // Если очередь не для "Така", ждем
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println("Так");
            tickTurn = true;  // Смена очередности
            lock.notify();  // Будим другой поток
        }
    }

    public static void main(String[] args) {
        TickTock tickTock = new TickTock();

        Thread tickThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                tickTock.tick();
                try {
                    Thread.sleep(1000);  // Задержка 1 секунда
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        Thread tockThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                tickTock.tock();
                try {
                    Thread.sleep(1000);  // Задержка 1 секунда
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        tickThread.start();
        tockThread.start();
    }
}
```

**Объяснение:**

1. **Общий объект lock**: используется для синхронизации потоков.
2. **Флаг tickTurn**: управляет порядком вывода "Тик" и "Так".
3. Потоки вызывают метод `tick()` или `tock()`, проверяя флаг `tickTurn`.
4. Потоки используют `wait()` для ожидания своей очереди и `notify()` для пробуждения другого потока.
5. Потоки выводят "Тик" или "Так" с задержкой в 1 секунду (`Thread.sleep(1000)`), обеспечивая интервал между выводами.

Код выводит:

```
Тик
Так
Тик
Так
Тик
Так
Тик
Так
Тик
Так
```
{{< /details >}}
