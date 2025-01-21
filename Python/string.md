
```python

word = 'Hello'

```

Строка в Python - это массив символов, к которому можно обращаться по индексу:


```python

print(word[3])  # Обращение к символу с индексом 3

print(word[-2]) # Обращение к символу с индексом -2 (с конца)

```

  
## Важный момент

  
**TypeError: 'str' object does not support item assignment**  

Строки нельзя редактировать напрямую. Попытка изменить символ вызовет ошибку:


```python

word[3] = 'r'  # Это вызовет ошибку

```

  
### Как изменить строку?


Можно преобразовать строку в список, изменить нужные элементы, а затем вернуть список обратно в строку:

  

```python

word_ls = list(word)  # Преобразуем строку в список

print(word_ls)        # Выводим список

word_ls[3] = 'r'     # Меняем символ с индексом 3

print(word_ls)       # Смотрим изменения


# Приводим список к строке

new_word = ''.join(word_ls)

print(new_word)      # Выводим новую строку

```

  

## Строка как частный случай кортежа


Строка - это неизменяемый список. 
Ниже приведены примеры поиска подстрок в строке:


```python

base_str = "Hello! It's me Mario!"

# Поиск строки в подстроке - возвращает True, если данная строка есть

word = 'Hello'  # Ищем 'Hello'

print(word in base_str)  # True

word = 'me'  # Ищем 'me'

print(word in base_str)  # True

word = 'box'  # Ищем 'box'

print(word in base_str)  # False

```

  

## Срезы строк

Мы можем применять срезы к строкам:

```python

print(base_str[2:10])     # Срез от 2 до 10

print(base_str[0:10:2])   # Срез от 0 до 10 с шагом 2

```

## Вызов методов для строк

  

Методы строк позволяют выполнять различные проверки:


```python

# Пример использования метода isalnum

print(base_str.isalnum())          # Проверяем, состоит ли строка только из букв и цифр

base_str = '123hello'

print(base_str.isalnum())          # True

base_str = '123'

print(base_str.isalnum())          # True

  

# Проверка на различные критерии

base_str = 'CAPS LOCK'

print(base_str.isupper())          # True

base_str = 'caps lock'

print(base_str.isupper())          # False

```

  

## Подсчет символов

  
Методы также могут использоваться для подсчета вхождений символов:
  

```python

base_str = 'Hello moto'

print(base_str.count('o'))      # Считаем количество букв 'o'

```