# Итераторы и Генераторы для Вложенных Списков в Python

Этот проект демонстрирует реализацию итераторов и генераторов для работы с вложенными списками в Python. Он включает в себя следующие компоненты:

1. **FlatIterator**: Итератор для плоских списков.
2. **flat_generator**: Генератор для плоских списков.
3. **FlatIteratorNested**: Итератор для списков с любым уровнем вложенности.
4. **flat_generator_nested**: Генератор для списков с любым уровнем вложенности.

## Содержание

- [Описание](#описание)
- [Требования](#требования)
- [Установка](#установка)
- [Использование](#использование)
  - [1. FlatIterator для плоских списков](#1-flatiterator-для-плоских-списков)
  - [2. flat_generator для плоских списков](#2-flat_generator-для-плоских-списков)
  - [3. FlatIteratorNested для вложенных списков](#3-flatiteratornested-для-вложенных-списков)
  - [4. flat_generator_nested для вложенных списков](#4-flat_generator_nested-для-вложенных-списков)
- [Тестирование](#тестирование)

## Описание

Проект реализует итераторы и генераторы, которые позволяют обрабатывать списки списков, возвращая их элементы в плоском виде. Это полезно для обхода вложенных структур данных.

- **FlatIterator** и **flat_generator** предназначены для работы с плоскими списками (списки, содержащие другие списки, но без дальнейшей вложенности).
- **FlatIteratorNested** и **flat_generator_nested** могут обрабатывать списки с любым уровнем вложенности.

## Требования

- Python 3.6 или выше

## Установка

1. **Клонируйте репозиторий:**

    ```bash
    git clone https://github.com/Shadowgraph-1/Netology_dz_Iter.git
    ```

2. **Перейдите в директорию проекта:**

    ```bash
    cd Netology_dz_Iter
    ```

## Использование

### 1. FlatIterator для плоских списков

Итератор, который принимает список списков и возвращает их плоское представление.

```python
class FlatIterator:

    def __init__(self, list_of_list):
        self.list_of_list = list_of_list
        self.outer_iter = iter(self.list_of_list)
        self.inner_iter = iter([])

    def __iter__(self):
        return self

    def __next__(self):
        try:
            return next(self.inner_iter)
        except StopIteration:
            self.inner_iter = iter(next(self.outer_iter))
            return next(self)
```

## 2. flat_generator для плоских списков

Генератор, который принимает список списков и возвращает их плоское представление.

```python

def flat_generator(list_of_lists):
    for sublist in list_of_lists:
        for item in sublist:
            yield item
```

## 3. FlatIteratorNested для вложенных списков

Итератор, обрабатывающий списки с любым уровнем вложенности.

```python

class FlatIteratorNested:

    def __init__(self, list_of_list):
        self.stack = [iter(list_of_list)]

    def __iter__(self):
        return self

    def __next__(self):
        while self.stack:
            try:
                current = next(self.stack[-1])
                if isinstance(current, list):
                    self.stack.append(iter(current))
                else:
                    return current
            except StopIteration:
                self.stack.pop()
        raise StopIteration
```

## 4. flat_generator_nested для вложенных списков

Генератор, обрабатывающий списки с любым уровнем вложенности.

```python

def flat_generator_nested(list_of_list):
    stack = list_of_list[::-1]
    while stack:
        current = stack.pop()
        if isinstance(current, list):
            stack.extend(current[::-1])
        else:
            yield current

```

## Тестирование

Каждый компонент имеет свой тестовый метод (test_1, test_2, test_3, test_4), который проверяет корректность работы соответствующего итератора или генератора.
Пример запуска

    Запустите все тесты:

    ```bash

python neto.py
```

При успешном прохождении тестов вы увидите следующие сообщения:

Test 1 passed.
Test 2 passed.
Test 3 passed.
Test 4 passed.

Если какой-либо тест не пройдет, будет выброшено исключение AssertionError.