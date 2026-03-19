# Лабораторная работа №3

## Данные о работе
- **Название:** Построение бинарного дерева (рекурсивный вариант)
- **Дисциплина:** Программирование на Python

## Цель работы
Построить бинарное дерево рекурсивным методом и исследовать другие структуры, в том числе доступные в модуле collections в качестве контейнеров для хранения структуры бинарного дерева.

## Код программы
```python
import unittest


def gen_bin_tree(height, root):
    """Функция gen_bin_tree принимает значения высоты дерева (height) и его корня (root) и строит по ним бинарное дерево, согласно формулам для левой и правой ветки (left, right)"""
    if isinstance(root, int) and isinstance(height, int):
        if root == 0:
            return None
        """При нулевом корне возвращается None"""
        if height >= 1:
            left = root+3
            right = root*2
            """Формулы для левых и правых веток"""
        else:
            return None
        """При нулевой высоте возвращается None"""
        if isinstance(left, int) and isinstance(right, int):
            if height - 1:
                return {root: [gen_bin_tree(height-1, left), gen_bin_tree(height-1, right)]}
            """Возвращает дерево в виде словаря"""
            return {root: []}
        """Выход из рекурсии"""
    else:
        return None
    """При корне или высоте не int возвращает None"""


class Test_gen_bin_tree(unittest.TestCase):

    def test_standartr(self):
        self.assertEqual(gen_bin_tree(5, 1), {1: [{4: [{7: [{10: [{13: []}, {20: []}]}, {14: [{17: []}, {28: []}]}]}, {8: [{11: [{14: []}, {22: []}]}, {16: [{19: []}, {32: []}]}]}]}, {
                         2: [{5: [{8: [{11: []}, {16: []}]}, {10: [{13: []}, {20: []}]}]}, {4: [{7: [{10: []}, {14: []}]}, {8: [{11: []}, {16: []}]}]}]}]})

    def test_root_not_int(self):
        self.assertIsNone(gen_bin_tree(5, 1.1))

    def test_height_not_int(self):
        self.assertIsNone(gen_bin_tree(5.2, 1))

    def test_height0(self):
        self.assertIsNone(gen_bin_tree(0, 1))

    def test_root0(self):
        self.assertIsNone(gen_bin_tree(5, 0))


"""Класс проверяет gen_bin_tree при:
стандартных условиях
корне, не являющимся int
высоте, не являющейся int
нулевой высоте
нулевом корне"""


unittest.main(argv=[''], verbosity=2, exit=False)
```

## Результаты выполнения

**Пример работы для height=2, root=1:**
gen_bin_tree(2, 1) =
{1: [
{4: [
{7: []},
{8: []}
]},
{2: [
{5: []},
{4: []}
]}
]}


**Результаты тестов:**
test_height0 (main.Test_gen_bin_tree) ... ok
test_height_not_int (main.Test_gen_bin_tree) ... ok
test_root0 (main.Test_gen_bin_tree) ... ok
test_root_not_int (main.Test_gen_bin_tree) ... ok
test_standartr (main.Test_gen_bin_tree) ... ok

Ran 5 tests in 0.004s

OK

## Выводы
В ходе выполнения лабораторной работы:

- Реализовано рекурсивное построение бинарного дерева с использованием вложенных словарей
- Исследованы формулы генерации левых (root+3) и правых (root*2) потомков
- Обработаны граничные случаи: нулевая высота, нулевой корень, некорректные типы данных
- Написаны тесты для проверки корректности работы функции
- Проанализирована структура бинарного дерева как способ организации данных