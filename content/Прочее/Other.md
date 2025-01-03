+++
title = 'Другое'
weight = 1
bookFlatSection = true
+++

## Другое

#### 1. Инструменты для отладки приложения, течет память, все замирает - Visual VM, JMap, профилировщики

Когда приложение ведет себя ненормально (например, потребляет слишком много памяти, замирает или падает), важно понять причину. Для этого используются инструменты мониторинга и отладки.

 **1. VisualVM**

- **Что это:** Инструмент для мониторинга JVM (Java Virtual Machine), входящий в состав JDK.

 **2. JMap**

- **Что это:** Утилита для работы с состоянием памяти в JVM.

 **3. Профилировщики**

- **Что это:** Специализированные инструменты для анализа работы приложений.
- **Примеры:**
    - **YourKit**: Анализ потребления памяти, потоков и выполнения методов.
    - **JProfiler**: Профилирование производительности, памяти, потоков.
    - **Async Profiler**: Низкоуровневый инструмент для анализа нагрузки на CPU и JVM.
- **Когда использовать:** Если приложение замедляется, можно профилировать его выполнение и выявить "узкие места" — методы или участки кода, которые занимают много времени.

