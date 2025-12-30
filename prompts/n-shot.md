## Разработка

Напиши функцию на Python, которая преобразует строку в «змеиный регистр» (snake_case).
Примеры:
"HelloWorld" → "hello_world"
"userID" → "user_id"
"APIResponse" → "api_response"
Учитывай аббревиатуры и смешанный регистр.

## Рефакторинг
Улучши читаемость и производительность кода. Вот исходный вариант и пример того, как это можно сделать:

Исходный код:
```python
def calc(a,b,c):
    return a*b + c*2
```

Улучшенный код:
```python
def calculate_total(price_per_unit, quantity, discount):
    """Вычисляет итоговую стоимость с учётом скидки."""
    subtotal = price_per_unit * quantity
    total = subtotal + (discount * 2)
    return total
```

Теперь примени тот же подход к следующему коду:
```python
def f(x,y):
    for i in range(len(x)):
        if x[i] == y:
            return True
    return False
```