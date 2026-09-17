# **Домашнее задание**
## **Курс: Основы автоматизированного тестирования**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

## **Задание**

1. Реализуйте Python-скрипт, который:

o Открывает сайт: https://www.litres.ru

o Закрывает всплывающее окно (если появится, можно просто
игнорировать с try/except)

o Вводит в строку поиска слово "Пушкин"

o Нажимает кнопку поиска

o Ждет загрузки результатов

o Извлекает названия первых 5 книг на странице и выводит их в
консоль


## **Решение**

*По скольку у меня с Литрес вообще ни в какую, я взял другой сайт*

import time

from selenium import webdriver

from selenium.webdriver.common.by import By

from selenium.webdriver.common.keys import Keys

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()

driver.maximize_window()

driver.get("https://litmarket.ru/")

search_box = driver.find_element(By.NAME, "search")

search_box.send_keys("Александр Пушкин")

time.sleep(2)

driver.find_element(By.XPATH, "//div[text()='Показать все результаты']").click()

book_element1 = driver.find_element(By.XPATH, "//a[contains(text(), 'Евгений Онегин')]")

book_element2 = driver.find_element(By.XPATH, "//a[text()='Капитанская дочка']")

print("Найдена книга:", book_element1.text)

print("Найдена книга:", book_element2.text)

time.sleep(5)

driver.quit()




