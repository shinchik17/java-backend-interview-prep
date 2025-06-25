#### 47. Распределение загрузки на грузовики

**Условие задачи:**  
🔥 Реализовать метод `calcTrucks`, который принимает массив весов грузов, количество грузовиков и их максимальную грузоподъемность, распределяет грузы по грузовикам так, чтобы **максимально** загрузить их, и возвращает **суммарную недогруженность** (общая свободная емкость).

```java
/**
 * @param weights           массив весов грузов
 * @param trucksCount       количество грузовиков
 * @param truckMaxCapacity  максимальная грузоподъемность одного грузовика
 * @return суммарная недогруженность (truckCount*truckMaxCapacity - суммарный вес размещённых грузов)
 */
private int calcTrucks(int[] weights, int trucksCount, int truckMaxCapacity) {
    // TODO
}
```

{{< hint warning >}}  
**Спойлеры к решению**  
{{< /hint >}}

{{< details "Подсказки" close >}}  
💡 Эту задачу можно свести к многоконтейнерному упаковочному (bin packing) с целью минимизации пустого места.  
💡 Один из эффективных эвристических методов — **Best Fit Decreasing**:

- Сначала отсортировать грузы по убыванию веса.

- Затем для каждого груза найти тот грузовик (контейнер), где после загрузки останется минимальное, но неотрицательное, свободное место.  
  💡 Если груз никак не помещается ни в один имеющийся контейнер, его можно пропустить (или считать недогруженным целиком).  
  {{< /details >}}


{{< details "Решение" close >}}

```java
import java.util.Arrays;

public class TruckLoader {

    public static int calcTrucks(int[] weights, int trucksCount, int truckMaxCapacity) {
        // 1. Сортировка грузов по убыванию для Best Fit Decreasing
        Arrays.sort(weights);
        // переворачиваем массив
        for (int i = 0, j = weights.length - 1; i < j; i++, j--) {
            int tmp = weights[i];
            weights[i] = weights[j];
            weights[j] = tmp;
        }

        // 2. Массив текущей загрузки каждого грузовика
        int[] load = new int[trucksCount];
        // инициализируем нулями

        // 3. Для каждого груза ищем "best fit"
        for (int w : weights) {
            int bestIndex = -1;
            int minRemaining = Integer.MAX_VALUE;
            for (int i = 0; i < trucksCount; i++) {
                int remaining = truckMaxCapacity - (load[i] + w);
                if (remaining >= 0 && remaining < minRemaining) {
                    minRemaining = remaining;
                    bestIndex = i;
                }
            }
            if (bestIndex >= 0) {
                load[bestIndex] += w;
            }
            // иначе груз не помещается никуда — пропускаем
        }

        // 4. Считаем суммарную недогруженность
        int totalFree = 0;
        for (int i = 0; i < trucksCount; i++) {
            totalFree += (truckMaxCapacity - load[i]);
        }
        return totalFree;
    }

    public static void main(String[] args) {
        System.out.println(calcTrucks(new int[]{10, 100, 20, 30, 40, 10}, 4, 100)); // 90
        System.out.println(calcTrucks(new int[]{10, 100, 20, 30, 40}, 4, 100));    // 0
    }
}
```

📌 **Что важно помнить:**  
✅ Best Fit Decreasing — простая и быстрая эвристика, дающая хорошее приближение к оптимуму для bin-packing.  
✅ Сначала сортируем грузы по убыванию, затем размещаем каждый в самый подходящий контейнер, минимизируя остаток.  
✅ Вычисляем суммарное свободное место по всем грузовикам и возвращаем его как меру недогрузки.  
{{< /details >}}