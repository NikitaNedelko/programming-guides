Краткое описание, что такое `pytest` и `Coverage.py`, как с этим работать.

# 🛠️ **1. Установка**

Команды для установки инструментов:

``` terminal
pip install pytest pytest-cov
```

> [!TIP]
> Проверяем, установился ли pytest: 
> ``` terminal
> pytest --version
> ```


# 📂 **2. Структура проекта**

Чтобы тесты были удобными и понятными, нужно **правильно структурировать проект**.

Обычно используют такую схему:

```
project/                 # Корень проекта
│── src/                 # Исходный код приложения
│   ├── module1.py       # Модуль 1
│   ├── module2.py       # Модуль 2
│   └── __init__.py      
│
│── tests/               # Папка с тестами
│   ├── test_module1.py  # Тесты для module1.py
│   ├── test_module2.py  # Тесты для module2.py
│   └── __init__.py      
│
│── requirements.txt     # Список зависимостей
│── pytest.ini           # Конфигурация pytest
│── .gitignore           # Игнорируемые файлы
│── README.md            # Описание проекта
```

> [!IMPORTANT] 
> 📌 **Правила:**
> - Все тесты хранятся в **папке `tests/`**.
> - Названия тестовых файлов **начинаются с `test_`** (например, `test_module1.py`).
> - В тестовых файлах **каждая тест-функция начинается с `test_`**.


## 📝 **3. Написание тестов (пример)**

Пример кода с assert:

```python

# src/my_example.py
def create_dict(keys_values_list: list[str]) -> dict[str, str | None]:
    """Создаёт словарь из списка ключей и значений."""

    if len(keys_values_list) % 2 != 0:
        raise ValueError("Ошибка: нечетное количество элементов.")

    return dict(zip_longest(keys_values_list[::2], keys_values_list[1::2]))
```

```python

# tests/test_example.py
def test_create_dict():
    """Тесты для функции create_dict.
    Проверяет корректное создание словаря из списка.
    """

    assert create_dict(["a", "1", "b", "2"]) == {"a": "1", "b": "2"}

    with pytest.raises(ValueError, match="Ошибка: нечетное количество элементов."): create_dict(["a", "1", "b"])
```

> [!IMPORTANT] 
> 📌 **Важно:** 
> pytest **автоматически находит файлы и функции, начинающиеся с `test_`**.


### **3.1. Параметризованные тесты (один тест на много случаев)**

Чтобы не писать однотипные `assert`, можно использовать `@pytest.mark.parametrize`:

```python

import pytest
from src.my_example import get_value_from_dict

@pytest.mark.parametrize(
    "input_dict, key, expected",
    [
        ({"a": "1", "b": "2"}, "a", "1"),
        ({"a": "1", "b": "2"}, "b", "2"),
        ({"a": "1", "b": "2"}, "c", "Такого ключа нет в словаре"),
        ({"a": "1", "b": "2"}, "d", "Такого ключа нет в словаре"),
    ],
)

def test_get_value_from_dict(
    input_dict: dict[str, str | None], key: str, expected: str
):
    """Тесты для функции get_value_from_dict.

    Проверяет корректное получение значения по ключу.
    """

    assert get_value_from_dict(input_dict, key) == expected
```


### **3.2 Моки и заглушки (если функция зависит от пользовательского ввода, API, БД и т.д.)**

Если нужно подменить работу сложных зависимостей, можно использовать `unittest.mock`:

```python
from unittest.mock import patch
from src.my_example import main

@patch(
    "builtins.input",
    side_effect=[
        "key1 value1 key2 value2 key3",  # Первый input() -> ключи и значения
    ],
)

@patch("builtins.print")
def test_main_my_dict(mock_print: MagicMock, mock_input: MagicMock):
    """Тестируем ввод ключей и их значений."""

    with pytest.raises(SystemExit):  # Ожидаем, что main() вызовет sys.exit()
        main()
```


## 🚀 **4. Запуск тестов**

 В терминале проекта запускаем:

```bash
pytest
```

> [!NOTE] 
> 📌 **Важно:** 
> pytest **автоматически находит файлы и функции, начинающиеся с `test_`**.


### **4.1. Запуск тестов с покрытием кода (Coverage.py)**

Чтобы проверить покрытие кода тестами, необходимо запустить:

```bash
pytest --cov=.
```

Если нужен детальный HTML-отчёт:

```bash
pytest --cov=. --cov-report=html
```

После этого нужно открыть `htmlcov/index.html` в браузере.

> [!NOTE] 
> 📌 **Виды запуска:**
> 
> Если нужно запустить только один файл с тестами:
>
>```bash
>pytest tests/test_example.py
>```
>
> Если необходимо запустить только один тест в файле:
>
>```bash
>pytest tests/test_example.py::test_create_dict
>```


## **5. Автоматический запуск тестов при каждом коммите**

Чтобы тесты **автоматически запускались перед коммитом в Git**, можно настроить `pre-commit`:

1. Создай файл `.git/hooks/pre-commit`:
	
	``` bash
	#!/bin/sh
	pytest
	```
	  
2. Дай права на выполнение:
    
    ``` bash
    chmod +x .git/hooks/pre-commit
    ```

> [!IMPORTANT] 
> 📌 **Важно:** 
> Теперь **Git не позволит сделать коммит, если тесты не проходят**