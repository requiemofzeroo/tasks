# **Домашнее задание**
## **Курс: Введение в язык программирования Python**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

## **Задание 1**

Безопасный клон тестового набора
Бизнес-требование: Автотест перед запуском должен скопировать эталонный словарь настроек пользователя и изменить в нем права, не повредив исходный шаблон.
Что сделать в коде:
1. Создайте вложенный словарь: `base_user = {"id": 77, "permissions": {"read": True, "write": False}}`
2. Сделайте ГЛУБОКУЮ копию этого словаря с помощью `copy.deepcopy()`. Сохраните в переменную `test_user`.
3. В словаре `test_user` измените значение вложенного ключа `"write"` на `True`.
4. Выведите в консоль отдельно оригинальный статус `base_user["permissions"]["write"]` и тестовый статус `test_user["permissions"]["write"]`. Убедитесь, что оригинал остался равен `False`.

## **Решение**

import copy

base_user = {

    "id": 77,
    "permissions":
        {
            "read": True,
            "write": False,
        }
}

test_user = copy.deepcopy(base_user)

test_user["permissions"]["write"] = True

print(f"Оригинальный статус (base_user): {base_user['permissions']['write']}")

print(f'Тестовый статус (test_user): {test_user["permissions"]["write"]}')


## **Второе задание**

Парсер и фильтр каталога курсов от API
Бизнес-требование: Из API прилетел список доступных курсов. Нам нужно перебрать его в цикле, найти курсы, которые стоят строго меньше 5000 рублей И у которых в списке тегов (вложенный список!) присутствует тег `"QA"`.
Что сделать в коде:
1. Скопируйте структуру данных от API:
course_catalog = [
    {"title": "Java Developer", "price": 8000, "tags": ["dev", "backend"]},
    {"title": "QA Manual", "price": 3000, "tags": ["QA", "manual"]},
    {"title": "Python для автоматизаторов", "price": 4500, "tags": ["QA", "automation"]}
]
2. Напишите цикл `for` для перебора этого списка курсов.
3. Внутри цикла напишите составное условие `if`: если цена курса (`price`) < 5000 И при этом слово `"QA"` находится внутри вложенного списка тегов (`"QA" in course["tags"]`), то:
4. Выведите на экран f-строкой сообщение: `[MATCH] Найден подходящий курс: {title} за {price} руб.`

## **Решение**

course_catalog = [

    {"title": "Java Developer", "price": 8000, "tags": ["dev", "backend"]},
    {"title": "QA Manual", "price": 3000, "tags": ["QA", "manual"]},
    {"title": "Python для автоматизаторов", "price": 4500, "tags": ["QA", "automation"]}
]

for course in course_catalog:

    if course["price"] < 5000 and "QA" in course["tags"]:
        print(f'[MATCH] Найден подходящий курс: {course["title"]} за {course["price"]} руб')
