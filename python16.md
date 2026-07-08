# **Домашнее задание**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

**Цель задания:** Закрепить на практике навыки проектирования классов, работы с конструктором `__init__`, изменения внутренних атрибутов (состояния) объекта через его методы, а также освоить базовые принципы разделения кода по паттерну Page Object Model.
 
---
 
### 📋 Техническое задание (ТЗ)
 
Вам необходимо написать прототип системы, которая имитирует автоматизированный прогон тестов на странице оплаты сайта и собирает внутреннюю аналитику стабильности элементов UI. Задача разделена на две логические части: описание архитектуры страницы и написание сценария теста.
 
#### Часть 1: Описание инфраструктуры (Класс Страницы)
 
Создайте класс `PaymentPage`.
 
1. **Конструктор (`__init__`)** должен принимать два обязательных параметра: имя пользователя (`username`, строка) и его текущий баланс на карте (`balance`, число).
2. Также внутри конструктора должны автоматически создаваться следующие атрибуты объекта:
   * Словарь локаторов `locators` с тремя элементами: 
     `{"card_input": "#card-number", "pay_button": "#submit-payment", "success_alert": ".alert-success"}`
   * Словарь внутренней статистики `stats`, в котором изначально зафиксированы три счетчика: 
     `{"total_actions": 0, "successful_actions": 0, "failed_actions": 0}`
3. Напишите метод `interact(element_name, action_success=True)`. Этот метод имитирует действие автотеста с элементом верстки:
   * **Проверка конфигурации:** Если переданного `element_name` нет в словаре `locators`, метод должен вывести в консоль сообщение: `"[SYSTEM ERROR] Локатор для элемента '{element_name}' не найден в системе!"` и увеличить счетчик `"failed_actions"` в словаре `stats` на 1. После этого метод завершает работу (`return`).
   * **Учет действий:** Каждое легитимное взаимодействие (если элемент существует в словаре `locators`) должно увеличивать общий счетчик `"total_actions"` на 1.
   * **Успешный исход:** Если параметр `action_success=True` (элемент нашелся на странице и сработал), метод увеличивает счетчик `"successful_actions"` на 1 и выводит лог: `"[OK] Успешное действие с элементом '{element_name}'"`.
   * **Неуспешный исход:** Если параметр `action_success=False` (имитация сбоя UI, элемент не прогрузился), метод увеличивает счетчик `"failed_actions"` на 1 и выводит лог: `"[FAIL] Не удалось повзаимодействовать с элементом '{element_name}'"`.
4. Напишите метод `make_payment(amount)`. Он проверяет баланс пользователя (`self.balance`):
   * Если баланс позволяет совершить покупку (больше или равен значению `amount`), то метод уменьшает баланс пользователя на эту сумму, вызывает внутри себя метод `self.interact("pay_button", action_success=True)` и возвращает `True`.
   * Если средств на балансе недостаточно, метод выводит лог о нехватке денег, вызывает внутри себя метод `self.interact("pay_button", action_success=False)` и возвращает `False`.
 
#### Часть 2: Написание сценария тестирования (Скрипт Теста)
 
Ниже класса напишите исполняемый код, который симулирует последовательные действия робота-раннера:
 
1. Создайте объект страницы `PaymentPage` для пользователя `"Alex QA"` со стартовым балансом `5000` рублей.
2. Имитируйте успешный ввод данных карты: вызовите метод `interact("card_input", action_success=True)`.
3. Имитируйте ошибку конфигурации (тестировщик опечатался в коде): вызовите метод `interact("promo_field", action_success=False)`. Проверьте, как система отработает элемент, которого нет в словаре `locators`.
4. Запустите сценарий покупки на сумму `6000` рублей (проверка ветки с нехваткой денежных средств).
5. Запустите сценарий покупки на сумму `2000` рублей (проверка успешного списания средств).
6. В самом конце скрипта выведите на экран финальный баланс пользователя и итоговый словарь статистики `stats`. 
7. Через финальные условия `if` проверьте, что баланс в итоге стал равен `3000`, а общее количество сохраненных ошибок `"failed_actions"` равно `2`. Если всё рассчитано верно, выведите итоговое сообщение: `🎉 Архитектурный тест пройден успешно!`.

## **Решение**

class PaymentPage:
    def __init__(self, username, balance):
    
        self.username = username
        self.balance = balance
        self.locators = {
            "card_input": "#card-number",
            "pay_button": "#submit-payment",
            "success_alert": ".alert-success"
        }
        self.stats = {
            "total_actions": 0,
            "successful_actions": 0,
            "failed_actions": 0
        }

    def interact(self, element_name, action_success = True):
        if element_name not in self.locators:
            print(f"[SYSTEM ERROR] Локатор для элемента {element_name} не найден в системе!")
            self.stats["total_actions"] += 1
            return

        self.stats["total_actions"] += 1

        if action_success:
            self.stats["successful_actions"] += 1
            print(f'[OK] Успешное действие с элементом {element_name}')
        else:
            self.stats["failed_actions"] += 1
            print(f"[FAIL] Не удалось повзаимодействовать с элементом '{element_name}'")

    def make_payment(self, amount):
        if self.balance >= amount:
            self.balance -= amount
            self.interact("pay_button", action_success=True)
            return True
        else:
            print(
                f"[SYSTEM] Недостаточно средств на балансе пользователя {self.username}. Текущий баланс: {self.balance}")
            self.interact("pay_button", action_success=False)
            return False

payment_page = PaymentPage("AlexQA", 5000)

payment_page.interact("card_input", action_success=True)

payment_page.interact("promo_field", action_success=False)

payment_page.make_payment(6000)

payment_page.make_payment(2000)

print(f'Финальный баланс: {payment_page.balance} руб.')

print(f'Статистика stats: {payment_page.stats}')

if payment_page.balance == 3000 and payment_page.stats["failed_actions"] == 2:

    print('🎉 Архитектурный тест пройден успешно!')
else:

    print('Тест провален.')














