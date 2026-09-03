# **Домашнее задание**
## **Курс: Основы автоматизированного тестирования**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

## **Задание**
В лекции рассматривались тесты для операции сложения с использованием
параметризации и позитивных/негативных кейсов. Теперь выполните аналогичную
задачу для операций умножения и деления. Реализуйте тесты с применением
возможностей Pytest, включая параметризацию, проверку исключений и
маркировку (@pytest.mark).


## Решение:

import pytest

@pytest.mark.parametrize('first_number, second_number, expected_result', [
    (10, 10, 100),
    
    (5, 5, 25),
    
    (5, 90, 450)
])

def test_multiply(first_number, second_number, expected_result):

    assert first_number * second_number == expected_result


@pytest.mark.parametrize('first_number, second_number, expected_result', [
    (10, 5, 2),
    
    (50, 2, 25),
    
    (81, 9, 9)
])

def test_divide(first_number, second_number, expected_result):

    assert first_number / second_number == expected_result


