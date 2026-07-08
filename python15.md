# **Домашнее задание**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

 
Коллеги, ваша задача — создать дома архитектурно правильный мини-проект автоматизации. В папке домашнего задания должно быть строго 3 файла:
 
📂 Файл 1: `environment.py` (Конфиг стенда)
Создайте в нем две глобальные переменные:
1. `TARGET_URL = "https://qa.ru"`
2. `BENCHMARK_LIMIT = 2.0` (максимально допустимое время ответа сервера в секундах).
 
📂 Файл 2: `tools.py` (Хелперы генерации и вычислений)
1. Импортируйте встроенный модуль `math`.
2. Напишите функцию `calculate_final_price(raw_price, discount_percent)`. Она должна рассчитывать цену со скидкой: `raw_price - (raw_price * discount_percent / 100)`.
3. Округлите получившуюся цену строго ВВЕРХ с помощью функции `math.ceil()`.
4. Верните (`return`) округленное целое число наружу.
 
📂 Файл 3: `api_test_suite.py` (Файл запуска автотестов)
1. Импортируйте встроенный модуль `time`.
2. Импортируйте настройки из `environment.py`.
3. Импортируйте функцию расчета цены из `tools.py`.
4. Напишите сценарий: 
   • Выведите лог старта: `[INIT] Отправляем запрос на эндпоинт: {TARGET_URL}/calculate`.
   • Зафиксируйте стартовое время перед вычислением: `start = time.time()`.
   • Вызовите функцию `calculate_final_price`, передав туда цену 1500 и скидку 12.5%. Результат напечатайте в консоль.
   • Имитируйте задержку сети, поставив принудительный сон `time.sleep(1.2)`.
   • Зафиксируйте финишное время: `end = time.time()`.Рассчитайте длительность теста: `duration = end - start`.
   • Напишите проверку `if...else`: если `duration` строго меньше или равно лимиту `BENCHMARK_LIMIT` из конфига, выведите: `[PASS] Скорость ответа API в пределах нормы: {duration:.2f} сек.`. Иначе выведите: `[FAIL] Сервер тормозит! Ответ шел {duration:.2f} сек.`.


## **Файл 1 - environment.py **

TARGET_URL = "https://qa.ru"

BENCHMARK_LIMIT = 2.0

## **Файл 2 -tools.py**

import math

def calculate_final_price(raw_price, discount_percent):

    discount_price = raw_price * discount_percent / 100
    ceil_price = math.ceil(raw_price)
    return ceil_price

## **Файл 3 - api_test_suite.py**

import time

from environment import TARGET_URL, BENCHMARK_LIMIT

from tools import calculate_final_price

print(f'[INIT] Отправляем запрос на эндпоинт: {TARGET_URL}/calculate')

start = time.time()

calculate_final_price(1500, 12.5)

time.sleep(1.2)

end = time.time()

duration = end - start

if duration < BENCHMARK_LIMIT:

    print(f'[PASS] Скорость ответа API в пределах нормы: {duration:.2f} сек.')
else:

    print(f'[FAIL] Сервер тормозит! Ответ шел {duration:.2f} сек.')



 
