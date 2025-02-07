Множества — это контейнеры, которые хранят уникальные элементы.

## Примеры использования множеств

### Пример 1: Создание множества из строки

```python
word = 'Hello'
set_word = set(word)
print(set_word)

word = "Gibiskus"
set_word = set(word)
print(set_word)
```

### Количество уникальных элементов

Можно подсчитать количество уникальных элементов:

```python
print(len(set_word))
```

### Вывод отсортированного множества

#### В обратном порядке

```python
print(sorted(set_word, reverse=True))
```

#### В прямом порядке

```python
print(sorted(set_word))
```

### Найдем расхождение между множествами

Выведем множество, в котором нет общих элементов между `number1` и `set_digits`:

```python
set_digits = set('0123456789')
number1 = set('1239')
print(set_digits.difference(number1))
```

### Пересечение множеств

Покажет множество совпадающих элементов:

```python
print(set_digits.intersection(number1))
```

### Проверка на отсутствие общих элементов

```python
print(set_digits.isdisjoint(number1))
```

### Добавление и удаление элементов

Добавление элемента:

```python
number1.add('4')
```

Удаление элемента:

```python
number1.remove('9')
```

Очистка множества:

```python
number2.clear()
```