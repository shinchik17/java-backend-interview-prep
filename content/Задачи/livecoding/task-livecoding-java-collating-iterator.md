#### 60. Итератор объединённого упорядоченного обхода двух источников

Есть Java интерфейс итератор, который бегает по коллекции. У него 2 метода. hasNext() возвращает есть ли следующий элемент для итерирования. next() возвращает следующий элемент и сдвигает итератор. У нас есть 2 итератора, которые упорядочены по возрастанию, в соответствии с компаратором, который мы знаем. Задача: написать один итератор, который возьмет два имеющихся итератора и пройдется по их общему набору в том же их отсортированном порядке. Реализовать методы hasNext() и next(). Пример приведен перед кодом. Можно добавлять новые поля. Только не меняем контракт интерфейса. Хотим константную сложность по памяти, то есть нельзя сначала считать оба итератора в память

```java
/*
        Принимает на вход два итератора a и b, упорядоченных по возрастанию
        в соответствии с некоторым компаратором.
        Написать итератор, который пройдет по общему набору из элементов a и b
        в порядке возрастания.
        Предполагаем, что все элементы не null.

        Пример:
        a проходит по {1, 3}
        b проходит по {2, 3, 5, 6}
        comparator -- обычное сравнение целых чисел
        Тогда CollatingIterator делает обход в следующем порядке:
        1 -> 2 -> 3 -> 3 -> 5 -> 6
*/

public class CollatingIterator<E> implements Iterator<E> {

    private final Iterator<E> a;
    private final Iterator<E> b;
    private final Comparator<E> comparator;

    public CollatingIterator(Iterator<E> a, Iterator<E> b, Comparator<E> comparator) {
        this.a = a;
        this.b = b;
        this.comparator = comparator;
    }

    @Override
    public boolean hasNext() {
        return false;
    }

    @Override
    public E next() {
        return null;
    }
}
```


{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Храним **по одному** «взгляду вперёд» (`nextA`, `nextB`) для каждого итератора.  
💡 При каждом вызове `next()` сравниваем эти «закэшированные» элементы и возвращаем меньший, затем **однократно** продвигаем соответствующий итератор.  
💡 Метод `hasNext()` просто проверяет, есть ли что вернуть из любого из источников.  
💡 Память — **константная**: храним максимум два элемента.  
{{< /details >}}

{{< details "Решение" close >}}

```java
import java.util.Comparator;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class CollatingIterator<E> implements Iterator<E> {

    private final Iterator<E> a;
    private final Iterator<E> b;
    private final Comparator<E> comparator;

    // Предварительно подгруженные элементы из каждого итератора
    private E nextA;
    private E nextB;
    private boolean hasNextA;
    private boolean hasNextB;

    public CollatingIterator(Iterator<E> a, Iterator<E> b, Comparator<E> comparator) {
        this.a = a;
        this.b = b;
        this.comparator = comparator;
        // Инициализируем первый элемент каждого итератора
        advanceA();
        advanceB();
    }

    @Override
    public boolean hasNext() {
        return hasNextA || hasNextB;
    }

    @Override
    public E next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        E result;
        if (hasNextA && hasNextB) {
            // Оба источника имеют элемент — выбираем меньший по компаратору
            if (comparator.compare(nextA, nextB) <= 0) {
                result = nextA;
                advanceA();
            } else {
                result = nextB;
                advanceB();
            }
        } else if (hasNextA) {
            result = nextA;
            advanceA();
        } else { // only B remains
            result = nextB;
            advanceB();
        }
        return result;
    }

    // Вспомогательный метод для подгрузки следующего элемента из итератора a
    private void advanceA() {
        if (a.hasNext()) {
            nextA = a.next();
            hasNextA = true;
        } else {
            nextA = null;
            hasNextA = false;
        }
    }

    // Вспомогательный метод для подгрузки следующего элемента из итератора b
    private void advanceB() {
        if (b.hasNext()) {
            nextB = b.next();
            hasNextB = true;
        } else {
            nextB = null;
            hasNextB = false;
        }
    }
}
````


{{< /details >}}