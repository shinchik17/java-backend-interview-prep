#### 34. Реализация паттерна Singleton в Java


**Условие задачи:**  
🔥 Напишите реализацию паттерна Singleton в Java.



{{< hint warning >}} **Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}} 
💡 Используй **private конструктор**, чтобы предотвратить создание экземпляров класса извне.  
💡 Применяй **double-checked locking** с ключевым словом `volatile` для обеспечения потокобезопасности и избежания избыточной синхронизации.  
💡 Метод `getInstance()` возвращает единственный экземпляр класса, гарантируя создание объекта только при первом обращении. {{< /details >}}

{{< details "Решение" close >}}

```java
public class Singleton {
    // volatile гарантирует, что изменения переменной instance видны всем потокам
    private static volatile Singleton instance;

    // Приватный конструктор предотвращает создание экземпляров извне
    private Singleton() {
    }

    // Метод для доступа к единственному экземпляру с использованием double-checked locking
    public static Singleton getInstance() {
        if (instance == null) { // Первичная проверка без синхронизации
            synchronized (Singleton.class) {
                if (instance == null) { // Вторая проверка внутри синхронизированного блока
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
    
    // Пример метода, который демонстрирует работу с Singleton
    public void doSomething() {
        System.out.println("Singleton instance is working!");
    }
    
    // Пример использования
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        singleton.doSomething(); // Вывод: Singleton instance is working!
    }
}
```

📌 **Что улучшилось?**  
✅ **Потокобезопасность:** Использование `volatile` и блока `synchronized` с двойной проверкой гарантирует корректную работу в многопоточной среде.  
✅ **Контроль создания экземпляра:** Приватный конструктор предотвращает создание дополнительных экземпляров класса.  
✅ **Эффективность:** Double-checked locking позволяет избежать блокировки при каждом вызове `getInstance()` после инициализации. {{< /details >}}