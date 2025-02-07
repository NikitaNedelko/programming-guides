Краткое описание, что такое `pytest` и `Coverage.py`, зачем они нужны.

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

> [!IMPORTANT] 📌 **Правила:**
> - Все тесты хранятся в **папке `tests/`**.
> - Названия тестовых файлов **начинаются с `test_`** (например, `test_module1.py`).
> - В тестовых файлах **каждая тест-функция начинается с `test_`**.

---

## **3. Написание тестов (пример)**

Пример кода:

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

    with pytest.raises(ValueError, match="Ошибка: нечетное количество элементов."):
        create_dict(["a", "1", "b"])
```

📌 **Важно:** pytest **автоматически находит файлы и функции, начинающиеся с `test_`**.

## **3. Запуск тестов**

Открываешь терминал в корне проекта и запускаешь:

```bash
pytest
```

🔥 Pytest сам найдёт тесты в `tests/` и запустит их.

> [!IMPORTANT] 📌 **Важно:** 
> pytest **автоматически находит файлы и функции, начинающиеся с `test_`**.


## **4. Запуск тестов с покрытием кода (Coverage.py)**

Если хочешь проверить покрытие кода тестами:

```bash
pytest --cov=.
```

Если нужен детальный HTML-отчёт:

```bash
pytest --cov=. --cov-report=html
```

📌 После этого открой `htmlcov/index.html` в браузере.

> [!NOTE]  
> Если хочешь запустить только один файл с тестами:
>
>```bash
>pytest tests/test_math_operations.py
>```

> [!NOTE]  
> Если хочешь запустить только один тест в файле:
>
>```bash
>pytest tests/test_math_operations.py::test_add
>```

## **6. Параметризованные тесты (один тест на много случаев)**

Чтобы не писать однотипные `assert`, можно использовать `@pytest.mark.parametrize`:

```python
import pytest
from src.math_operations import add

@pytest.mark.parametrize("x, y, expected", [
    (2, 3, 5),
    (-1, 1, 0),
    (0, 0, 0),
    (100, 200, 300)
])
def test_add(x, y, expected):
    assert add(x, y) == expected
```

📌 **Преимущество:** меньше кода – больше тестов! 🚀

---

## **7. Моки и заглушки (если функция зависит от API, БД и т.д.)**

Если нужно подменить работу сложных зависимостей (например, API-запросов), можно использовать `unittest.mock`:

```python
from unittest.mock import patch
import src.api_client as api

@patch("src.api_client.get_data")
def test_api_call(mock_get_data):
    mock_get_data.return_value = {"status": "ok"}
    
    response = api.get_data()
    assert response["status"] == "ok"
```

📌 **Когда использовать?** Когда тестируемая функция делает запрос в БД, API или файловую систему.



## **8. Автоматический запуск тестов при каждом коммите**

Чтобы тесты **автоматически запускались перед коммитом в Git**, можно настроить `pre-commit` хук:

1. Создай файл `.git/hooks/pre-commit`:
    
    ```bash
    #!/bin/sh
    pytest
    ```
    
2. Дай права на выполнение:
    
    ```bash
    chmod +x .git/hooks/pre-commit
    ```
    

Теперь **Git не позволит сделать коммит, если тесты не проходят!** 🔥



## **Вывод**

✅ Тесты храним в `tests/`.  
✅ Файлы тестов называем `test_*.py`.  
✅ Запускаем тесты через `pytest`.  
✅ Покрытие кода тестами проверяем через `pytest --cov`.  
✅ Автоматизируем с `pre-commit`.

Если хочешь, могу помочь с настройкой на твоём проекте! 🚀