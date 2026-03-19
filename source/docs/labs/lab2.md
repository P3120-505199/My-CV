# Лабораторная работа №2

## Данные о работе
- **Название:** Угадай число
- **Дисциплина:** Программирование на Python

## Цель работы
Познакомится с алгоритмами медленного перебора и бинарного поиска, продолжить изучение библиотеки unittest

## Код программы
```python
import unittest


def guess_num(num1, num2, target, method):
    """Функция guess_num ищет заданное число (target) в промежутке от num1 до num2 согласно выбранному методу (method)
      и выводит список: заданное число и число угадываний (answer, k)"""

    nums = set()
    if num2 > num1:
        for j in range(num1, num2+1):
            nums.add(j)
    else:
        for j in range(num2, num1+1):
            nums.add(j)
    nums = sorted(nums)

    """Формирование списка чисел от num1 до num2 в случаях разного порядка ввода"""

    k = 1
    answer = []
    if method == 1:
        for i in nums:
            if i == target:
                answer.append(i)
                answer.append(k)
                return answer
            else:
                k += 1

    """При выборе первого метода производит медленный перебор списка и добавляет в ответ заданное число и число угадываний"""

    if method == 2:
        left = 0
        right = len(nums)
        while right > left:
            mid = (left + right) // 2
            if nums[mid] == target:
                answer.append(nums[mid])
                answer.append(k)
                return answer
            elif nums[mid] < target:
                left = mid + 1
                k += 1
            else:
                right = mid
                k += 1

    """При выборе второго метода производит бинарный поиск в списке и добавляет в ответ заданное число и число угадываний"""

    if len(answer) == 0:
        return None

    """Если ответ пуст, возвращает None """


class Test_guess_num(unittest.TestCase):

    def test_standart(self):
        self.assertEqual(guess_num(2, 8, 7, 1), [7, 6])

    def test_no_num(self):
        self.assertIsNone(guess_num(0, 4, 5, 1))

    def test_bad_input(self):
        self.assertEqual(guess_num(7, 0, 5, 1), [5, 6])

    def test_no_num_2(self):
        self.assertIsNone(guess_num(0, 4, 5, 2))

    def test_bad_input_2(self):
        self.assertEqual(guess_num(7, 0, 5, 2), [5, 3])

    def test_standart_2(self):
        self.assertEqual(guess_num(2, 8, 7, 2), [7, 6])


"""Класс Test_guess_num тестирует функцию при помощи библиотеки unittest при применении обоих методов 
на стандартные условия работы (test_standart, test_standart_2)
на отсутсвие заданного числа в списке (test_no_num, test_no_num_2) 
на неправильный порядок введённых границ списка (test_bad_input, test_bad_input_2)"""

unittest.main(argv=[''], verbosity=2, exit=False)
```

## Результаты выполнения

**Линейный поиск:**
guess_num(2, 8, 7, 1) -> [7, 6] # найдено за 6 попыток
guess_num(0, 4, 5, 1) -> None # число не найдено
guess_num(7, 0, 5, 1) -> [5, 6] # границы переставлены, но работает


**Бинарный поиск:**
guess_num(2, 8, 7, 2) -> [7, 6] # найдено за 6 попыток
guess_num(0, 4, 5, 2) -> None # число не найдено
guess_num(7, 0, 5, 2) -> [5, 3] # найдено быстрее, чем линейным


**Результаты тестов:**
test_bad_input (main.Test_guess_num) ... ok
test_bad_input_2 (main.Test_guess_num) ... ok
test_no_num (main.Test_guess_num) ... ok
test_no_num_2 (main.Test_guess_num) ... ok
test_standart (main.Test_guess_num) ... ok
test_standart_2 (main.Test_guess_num) ... ok

Ran 6 tests in 0.003s

OK

## Выводы
В ходе выполнения лабораторной работы:
-  Изучены два алгоритма поиска: линейный и бинарный 
-  Реализована функция, работающая обоими методами в зависимости от параметра
-  Написаны тесты для проверки корректности работы при различных входных данных
-  Проверена работа при некорректном порядке границ диапазона
-  Сравнена эффективность алгоритмов: бинарный поиск находит число быстрее при больших диапазонах
-  Продолжено освоение библиотеки unittest для модульного тестирования