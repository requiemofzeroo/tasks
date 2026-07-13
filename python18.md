# **Домашнее задание**
### **Выполнил студент группы QA511: Курбатов Максим Вадимович**

### 📋 Техническое задание:
 
1️⃣ **Создайте класс `BasePage` (Уровень 1):**
* Конструктор `__init__` должен принимать строку `browser` и сохранять её в публичный атрибут.
* Напишите метод `click_element(locator)`. Он должен выводить лог: `"[BASE] Выполнен клик по элементу: {locator}"`.
 
2️⃣ **Создайте класс `SecurePage` (Уровень 2, наследуется от `BasePage`):**
* Конструктор `__init__` должен принимать параметры `browser` (для родителя) и строку `auth_cookie`.
* Внутри конструктора вызовите `super().__init__(browser)`. Атрибут `auth_cookie` сохраните как приватный (`__auth_cookie`).
* Создайте геттер `get_cookie(self)`, возвращающий значение этой приватной переменной.
* **Переопределите (полиморфизм)** метод `click_element(locator)`. Он должен проверять: если длина куки в `__auth_cookie` больше 0, то сначала выводится лог `"[SECURE] Проверка авторизации: сессия активна"`, а затем вызывается родительский клик через `super().click_element(locator)`. Если кука пустая, клик не выполняется, а выводится лог: `"[SECURE ERROR] Клик отклонен: пользователь не авторизован!"`.
 
3️⃣ **Создайте класс `ProfileSettingsPage` (Уровень 3, наследуется от `SecurePage`):**
* Конструктор `__init__` должен принимать параметры `browser`, `auth_cookie` и список строк `available_settings` (список доступных настроек, например `["change_avatar", "change_password"]`).
* Прокиньте необходимые параметры выше по цепочке через `super().__init__()`. Список настроек сохраните в приватный атрибут `__settings_list`.
* Напишите метод `toggle_setting(setting_name)`. Он должен проверить, есть ли переданная строка в приватном списке `__settings_list`. 
* Если настройка найдена, метод вызывает унаследованный метод `self.click_element(f"#btn-{setting_name}")`.
* Если настройки нет в списке, выводится лог: `"[SETTINGS ERROR] Опция '{setting_name}' недоступна в данном профиле!"`.
 
### 🎛️ Проверка в теле теста (напишите ниже классов):
1. Создайте объект класса `ProfileSettingsPage` для браузера `"Firefox"`, с валидной кукой `"SESSION_ID_XYZ888"` и списком настроек `["avatar", "privacy"]`.
2. Вызовите метод `toggle_setting("privacy")` (сценарий успешного прохождения всей цепочки наследования).
3. Вызовите метод `toggle_setting("delete_account")` (проверка сценария отсутствия настройки в конфигурации).
4. Создайте второй объект класса `ProfileSettingsPage` для браузера `"Chrome"`, но передайте ему **пустую куку** `""` и список настроек `["avatar"]`. Вызовите метод `toggle_setting("avatar")` и убедитесь, что сработала защита промежуточного уровня `SecurePage`.

## **Решение**

class BasePage:

    def __init__(self, browser):
        self.browser = browser

    def click_element(self, locator):
        print(f'[BASE] Выполнен клик по элементу: {locator}')

class SecurePage(BasePage):

    def __init__(self, browser, auth_cookie):
        super().__init__(browser)
        self.__auth_cookie = auth_cookie

    def get_cookie(self):
        return self.__auth_cookie

    def click_element(self, locator):
        if len(self.__auth_cookie) > 0:
            print('[SECURE] Проверка авторизации: сессия активна')
            super().click_element(locator)
        else:
            print("[SECURE ERROR] Клик отклонен: пользователь не авторизован!")

class ProfileSettingsPage(SecurePage):

    def __init__(self, browser, auth_cookie, available_settings):
        super().__init__(browser, auth_cookie)
        self.__settings_list = available_settings

    def toggle_settings(self, setting_name):
        if setting_name in self.__settings_list:
            self.click_element(f"#btn-{setting_name}")
        else:
            print(f"[SETTINGS ERROR] Опция '{setting_name}' недоступна в данном профиле!")


profile_set = ProfileSettingsPage('Firefox', 'SESSION_ID_XYZ888', ['avatar', 'privacy'])

profile_set.toggle_settings('privacy')

profile_set.toggle_settings('delete_account')

profile_set2 = ProfileSettingsPage('Chrome', "", ['avatar'])

profile_set2.toggle_settings('avatar')
