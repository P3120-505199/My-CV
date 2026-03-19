# Отчёт по лабораторной работе 4
# Выполнил Богун Андрей

---

## Исходный код fact_recursive и fact_iterative

```python
import timeit
import matplotlib.pyplot as plt
import random


def fact_recursive(n: int) -> int:
    """Рекурсивный факториал"""
    if n == 0:
        return 1
    return n * fact_recursive(n - 1)


def fact_iterative(n: int) -> int:
    """Нерекурсивный факториал"""
    res = 1
    for i in range(1, n + 1):
        res *= i
    return res


def benchmark(func, n, number=1, repeat=5):
    """Возвращает среднее время выполнения func(n)"""
    times = timeit.repeat(lambda: func(n), number=number, repeat=repeat)
    return min(times)



def main():
    # фиксированный набор данных
    random.seed(42)
    test_data = list(range(10, 300, 10))

    res_recursive = []
    res_iterative = []

    for n in test_data:
      res_recursive.append(benchmark(fact_recursive, n, repeat=10, number=5000))
      res_iterative.append(benchmark(fact_iterative, n, repeat=10, number=5000))

    # Визуализация
    plt.plot(test_data, res_recursive, label="Рекурсивный")
    plt.plot(test_data, res_iterative, label="Итеративный")
    plt.xlabel("n")
    plt.ylabel("Время (сек)")
    plt.title("Сравнение рекурсивного и итеративного факториала")
    plt.legend()
    plt.show()


if __name__ == "__main__":
    main()
```
## Запуск в разных средах с параметрами: repeat=10, number=5000

### Локальный запуск функций выдал следующий результат

![Локальный запуск]("C:\Users\user\Documents\GitHub\Itmo-studies\lab_4\Local.png")

### Запуск в Google Colab

![Colab]("C:\Users\user\Documents\GitHub\Itmo-studies\lab_4\Collab.png")

## Сравнение производительности

### Оба графика показывают одинаковую тенденцию: итеративный метод быстрее рекурсивного

### Форма кривых схожа: экспоненциальный рост времени выполнения с увеличением n

### Разрыв в производительности между двумя методами увеличивается с ростом n

| Параметр | Collab | Local | Ускорение |
|----------|-------:|------:|----------:|
| Среднее время | 0.085 | 0.034 | 2.50x |
| Максимальное | 0.270 | 0.110 | 2.45x |
| Минимальное | 0.002 | 0.001 | 2.00x |

## Ключевые выводы:

### 1. Производительность локальной среды выше
**Локальный компьютер в ~2.5 раза быстрее удалённого (Colab)**  
Это ожидаемо: удалённые среды имеют накладные расходы на сеть, виртуализацию

### 2. Относительная эффективность методов сохраняется
**Итеративный метод быстрее рекурсивного в обоих случаях:**
- **Colab**: в ~1.8 раза при n=300
- **Local**: в ~1.83 раза при n=300

Пропорция сохраняется, несмотря на разницу в абсолютных значениях

### 3. Характер роста времени
- **Рекурсивный метод**: более крутой рост (накладные расходы на вызовы функций, стек)
- **Итеративный метод**: более пологий рост (меньше накладных расходов)


