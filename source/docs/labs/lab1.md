# Лабораторная работа №1

## 📋 Данные о работе
- **Название:** Сумма двух
- **Дисциплина:** Программирование на Python

## Цель работы
Изучить библиотеку unittest в Python

## Код программы
```python
def twoSum(nums, target):
    ans = []

    if len(nums) < 2:
        return None

    for j in range(len(nums) - 1):
        for i in range(j + 1, len(nums)):
            if nums[j] + nums[i] == target:
                ans.append((j, i))

    if not ans:
        return None

    return min(ans)



import unittest


class TestMySolution(unittest.TestCase):

    def test_example_1(self):
        self.assertEqual(twoSum([2, 7, 11, 15], 9), (0, 1))

    def test_example_2(self):
        self.assertEqual(twoSum([3, 2, 4], 6), (1, 2))

    def test_example_3(self):
        self.assertEqual(twoSum([3, 3], 6), (0, 1))

    def test_same_lst_elems(self):
        self.assertEqual(twoSum([4, 4, 4], 8), (0, 1))
    
    def test_len1(self):
        self.assertIsNone(twoSum([1], 1))

    def test_len0(self):
        self.assertIsNone(twoSum([], 1))
    
    def test_float(self):
        self.assertEqual(twoSum([0.5, 3.14, 3, 2, 7.1], 5), (2, 3))

    def test_negatives(self):
        self.assertEqual(twoSum([-3, -1, 5, 4,  -42, -5], 9), (2, 3))
    

if __name__ == "__main__":
    unittest.main()
```

## Выводы
В ходе выполнения лабораторной работы:
- Изучена библиотека `unittest` для модульного тестирования в Python
- Реализована функция поиска пары элементов с заданной суммой
- Написаны тесты, покрывающие различные случаи (обычные, граничные, некорректные данные)
- Проверена работа с разными типами данных (int, float)