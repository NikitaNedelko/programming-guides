Краткое описание, что такое `pytest` и `Coverage.py`, зачем они нужны.

### **Как правильно организовать тесты в Python (pytest) в VS Code**

Чтобы тесты были удобными и понятными, нужно **правильно структурировать проект**.

---

## **1. Структура проекта**

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

📌 **Правила:**

- Все тесты хранятся в **папке `tests/`**.
- Названия тестовых файлов **начинаются с `test_`** (например, `test_module1.py`).
- В тестовых файлах **каждая тест-функция начинается с `test_`**.

---

## **2. Написание тестов (пример)**

Допустим, у тебя есть файл `src/math_operations.py`:

```python
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y
```

Теперь создадим тест для него в `tests/test_math_operations.py`:

```python
from src.math_operations import add, subtract

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0

def test_subtract():
    assert subtract(5, 3) == 2
    assert subtract(10, 5) == 5
```

📌 **Важно:** pytest **автоматически находит файлы и функции, начинающиеся с `test_`**.

---

## **3. Запуск тестов**

Открываешь терминал в корне проекта и запускаешь:

```bash
pytest
```

🔥 Pytest сам найдёт тесты в `tests/` и запустит их.

---

## **4. Запуск тестов с покрытием кода (Coverage.py)**

Если хочешь проверить покрытие кода тестами:

```bash
pytest --cov=src
```

Если нужен детальный HTML-отчёт:

```bash
pytest --cov=src --cov-report=html
```

📌 После этого открой `htmlcov/index.html` в браузере.

---

## **5. Фильтрация тестов**

> [!NOTE]  
> Если хочешь запустить только один файл с тестами:
>
>```bash
>pytest tests/test_math_operations.py
>```

Если хочешь запустить только один тест в файле:

```bash
pytest tests/test_math_operations.py::test_add
```

---

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

---

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

---

## **Вывод**

✅ Тесты храним в `tests/`.  
✅ Файлы тестов называем `test_*.py`.  
✅ Запускаем тесты через `pytest`.  
✅ Покрытие кода тестами проверяем через `pytest --cov`.  
✅ Автоматизируем с `pre-commit`.

Если хочешь, могу помочь с настройкой на твоём проекте! 🚀