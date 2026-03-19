# Лабораторная работа №5

## Данные о работе
- **Название:** Построение бинарного дерева (нерекурсивный вариант)
- **Дисциплина:** Программирование на Python

## Цель работы
Построить бинарное дерево нерекурсивным методом и исследовать другие структуры, в том числе доступные в модуле collections в качестве контейнеров для хранения структуры бинарного дерева. 

## Код программы
```python
import unittest
from collections import *


def gen_bin_tree(height, root, left_branch=lambda x: x*2, right_branch=lambda x: x+3):
    """Генерирует бинарное дерево в виде словаря нерекурсивным способом."""
    if root == 0:
        return None
    """Возвращает None, если корень равен нулю"""
    if height == 0:
        return None
    """Возвращает None, если высота равна нулю"""
    if type(height) != int or type(root) != int:
        return None
    """Возвращает None, если или высота или корень не являются int"""
    tree = {'value': root}
    """Создание корневого узла"""
    queue = deque()
    queue.append((tree, 1))
    """Создание двусторонней очереди"""
    while queue:
        current_node, current_height = queue.popleft()
        if current_height >= height:
            continue
        """Если достигла максимальной высоты, не добавляет потомков"""
        left_leaf = left_branch(current_node['value'])
        current_node['left'] = {'value': left_leaf}
        queue.append((current_node['left'], current_height+1))
        right_leaf = right_branch(current_node['value'])
        current_node['right'] = {'value': right_leaf}
        queue.append((current_node['right'], current_height+1))
        """Создание левого и правого потомков путём обновления значения корневого узла и отслеживания текущей высоты"""
    return tree


class Test_gen_bin_tree(unittest.TestCase):

    def test_standartr(self):
        self.assertEqual(gen_bin_tree(5, 1), {'value': 1, 'left': {'value': 2, 'left': {'value': 4, 'left': {'value': 8, 'left': {'value': 16}, 'right': {'value': 11}}, 'right': {'value': 7, 'left': {'value': 14}, 'right': {'value': 10}}}, 'right': {'value': 5, 'left': {'value': 10, 'left': {'value': 20}, 'right': {'value': 13}}, 'right': {'value': 8, 'left': {'value': 16}, 'right': {
                         'value': 11}}}}, 'right': {'value': 4, 'left': {'value': 8, 'left': {'value': 16, 'left': {'value': 32}, 'right': {'value': 19}}, 'right': {'value': 11, 'left': {'value': 22}, 'right': {'value': 14}}}, 'right': {'value': 7, 'left': {'value': 14, 'left': {'value': 28}, 'right': {'value': 17}}, 'right': {'value': 10, 'left': {'value': 20}, 'right': {'value': 13}}}}})

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

**Пример работы для height=3, root=2 с функциями по умолчанию:**
```python
gen_bin_tree(3, 2)
{
    "value": 2,
    "left": {
        "value": 4,
        "left": {
            "value": 8,
            "left": {"value": 16},
            "right": {"value": 11}
        },
        "right": {
            "value": 7,
            "left": {"value": 14},
            "right": {"value": 10}
        }
    },
    "right": {
        "value": 5,
        "left": {
            "value": 10,
            "left": {"value": 20},
            "right": {"value": 13}
        },
        "right": {
            "value": 8,
            "left": {"value": 16},
            "right": {"value": 11}
        }
    }
}
```

## Результаты тестов
test_height0 (__main__.Test_gen_bin_tree) ... ok
test_height_not_int (__main__.Test_gen_bin_tree) ... ok
test_root0 (__main__.Test_gen_bin_tree) ... ok
test_root_not_int (__main__.Test_gen_bin_tree) ... ok
test_standartr (__main__.Test_gen_bin_tree) ... ok

----------------------------------------------------------------------
Ran 5 tests in 0.003s

OK

## Выводы

В ходе выполнения лабораторной работы:

- Реализовано нерекурсивное построение бинарного дерева с использованием обхода в ширину (BFS)
- Исследована структура `deque` из модуля `collections` как эффективный контейнер для очереди
- Реализована гибкость через передачу lambda-функций для генерации потомков
- Проведено тестирование для различных граничных случаев