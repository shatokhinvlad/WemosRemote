# Wemos Remote

Wemos remote - проект для керування авто моделлю з додатку на телефоні. 
Керування реалізовано на процесорі ESP8266. Цей процесор компактний, має вбудований WIFI модуль і адаповане ядро під фреймворк Arduino. Прошивка реалізована у середовищі VisualStudio 2019 з використанням фреймворків:
* Arduino https://www.arduino.cc/en/Main/Software
* RemoteXY http://remotexy.com/
* ESP8266 https://github.com/esp8266/Arduino
* jQuery https://jquery.com/
* Bootstrap https://getbootstrap.com/

При необхідлності скетч можна редагувати у середовищі Arduino IDE.

# Основні можливості
## Можливості прошивки
* Керування через WIFI з телефона під кправлення Android/IOs (з допомогою бібліотеки RemotXY http://remotexy.com). Проект інтерфейсу програми можна знайти тут http://remotexy.com/ru/editor/321f7c2c5d592ddd85a15c3eff2505cf/
* Керування одним двигуном постійного струму
* Керування одним сервомотором
* Детекція режиму очікування підєднання з допомогою вбудованого світлодіода
* Імітація постановки/зняття з сигналізації під час підєднання/відєднання додатку телефона
* Автоматичне уввімкнення/вимкнення поворотів під час їзди
* Автоматичне включення стопсигналу під час зупинки на 2 секунди
* Автоматичне включення лампи заднього ходу під час руху назад
* Окрема кнопка для аварійної сигналізації
* Перключення освітлення Вимк->Стоянкові вогні->Освітлення

## Параметри, що налаштовуються
Прошивка має можливість налаштування.

Основні параметри:

* Назва точки доступу
* Пароль точки доступу
* Налаштування сервоприводу
  * Кут центрального положення сервомотора
  * Кут прайнього лівого положення
  * Кут крайнього правого положення
* Налаштування мотора
  * Мінімальне значення ШИМ для рушання з місця
* Налаштування світла
  * Яскравість основного світла
  * Яскравість габаритних стоянкоянкових вогнів
  * Тривалість включення стопсигналу після зупинки моделі


# З чого розпочати?
## Що необхідно?
Зробити радіокерування для моделі дуже просто.

Базовий набір:
* Завантажена з GitHub копія проекту
* Модуль, або відлагодочна плата на базі процесора ESP8266 (Наприклад ESP12, ESP12-S, Wemo D1 mini, або інша.)
* Драйвер двигуна
* Сервопривід
* Компютер з USB
* Веб переглядач для налаштування

Додаткові інструпенти, якщо ви захочете переробити прошивку під більш складні задачі:
* Arduino IDE
* Встановлена бібіліотека для ESP8266

## Прошивка базової конфігурації
У папці Tools є утиліта для прошивки та безпосередньо сам файл прошивки. Для більшості користувачів цього є цілком достатньо. Виконавши декілька простих кроків ви зможете перетворити плату у радіоапаратуру для керування моделями.

Покрокова інструкція
1. Підключаєте плату до USB вашого компютера.
2. Встановлюєте драйвера згідно з інструкціями виробника плати
3. Заходете у диспечер пристроїів і перевіряєти чи всі драйвера встанорвлено і ваша плата розпізнається системою. 
Запамятовуєте який номер порта отримала ваша плата
4. запускаєте Tools/upload.bat
5. Після старту - скрипт запитає номер порта до якого підєднано вашу плату
6. Чекаєте поки завершиться процез завантаження

Все плата прошита.

З цього моменту нею можна користуватись.

Якщо ж ви бажаєте змінити деякі налаштування (діапазон повороту сервоприводу, стартову швидкість, яскравість світла... тощо), то додатково необхідно завантажити в память контроллера модуль налаштувань

## Завантаження модуля налаштувань
На цьому кроці ви вже маєте підключену до вашого USB плату і знаєте який номер порта вона отримала.
Приступаємо
1. запускаєте Tools/setup.bat
2. Після старту - скрипт запитає номер порта до якого підєднано вашу плату
3. Чекаєте поки завершиться процез завантаження

Все плата поновлена.

## Налаштування
Для налаштування вам необхідно на телефоні підєжднатися до wifi мережі Wemos_00000000. (замість нулів буде серійний номер вашої плати)
Стандартний пароль 12345678. Ви можете його змінити за вашим бажанням.

Після підключення - відкриваєте web переглядач і переходете на адресу 192.168.4.1/ Це адреса для налаштувань.

## Підключення

1) Сервопривід Керуючий вивід => D5
2) Тяговий мотор Вхід А => D7, Вхід Б => D6
3) Головне світло D4
4) Лівий поврот D2
5) Правий поворот D1
6) Задній хід D3
7) Стопсигнал D8
8) Габарит D0
9) Бузер RX

