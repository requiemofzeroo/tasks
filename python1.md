# **Домашнее задание**
## **Курс: Введение в языкпрограммирования Python**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

## Первое задание:

**Пользователь вводит с клавиатурытри числа. Необходимо найти сумму чисел, произведение чисел. 
Результат вычислений вывести на экран**

first_number = int(input('Введите первое число: '))

second_number = int(input('Введите второе число: '))

third_number = int(input('Введите третье число: '))

summa = (first_number + second_number + third_number)

print(f'Результат: {summa}')

## Второе задание:

**Пользователь вводит с клавиатуры три числа. Первое
число — зарплата за месяц, второе число — сумма месячного платежа по кредиту в банке, третье число — задолженность за коммунальные услуги. Необходимо вывести
на экран сумму, которая останется у пользователя после
всех выплат.**

monthly_salary = int(input('Введите зарплату за месяц: '))

bank_payment = int(input('Введите сумму месячного платежа по кредиту: '))

debt_utilities = int(input('Введите задолженность за коммунальные услуги: '))

balance = monthly_salary - bank_payment - debt_utilities

print(f'Остаток: {balance}')

## Третье задание:

**Напишите программу, вычисляющую площадь ромба. Пользователь с клавиатуры вводит длину двух его
диагоналей.**

first_diagonal = int(input('Введите первую диагональ: '))

second_diagonal = int(input('Введите вторую диагональ: '))

square = (first_diagonal * second_diagonal) // 2

print(f'Площадь равна: {square}')

## Четвертое задание:

**Выведите на экран надпись To be or not to be на разных
строках.**

print("To be")

print("or not")

print("to be")

## Пятое задание:

**Выведите на экран надпись «Life is what happens when
you're busy making other plans» John Lennon на разных
строках.**

print("“Life is what happens")

print("      when")

print("         you’re busy making other plans”")

print("                                    John Lennon.")
