# Wemos Remote

Wemos remote - проект для керування авто моделлю з додатку на телефоні. 

Проект зроблено спеціально для моделеі ВАЗ2108 з Youtube каналу Евгений Былеев https://www.youtube.com/channel/UC27d-7p-ovLAEvgUijC3Dwg

Усі етапи переобки моделі:
* Частина 1 https://www.youtube.com/watch?v=yfO8KKdHDNg
* Частина 2 https://www.youtube.com/watch?v=fh4VSRUZ7cI
* Частина 3 https://www.youtube.com/watch?v=LJ8kn36iIjE
* Частина 4 https://www.youtube.com/watch?v=ulSTMTheBtc
* Частина 5 https://www.youtube.com/watch?v=QjuS22DsXOE&t=425s
* Частина 6 https://www.youtube.com/watch?v=itL2xLTIavY
* Частина 7 https://www.youtube.com/watch?v=qjkIQKs_uEk
* Частина 8 https://www.youtube.com/watch?v=_W1Ysf4-UWk
* Частина 9 Ще не вийшла....

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
* Імітація постановки/зняття з сигналізації під час підєднання/відєднання додатку телефона
* Автоматичне уввімкнення/вимкнення поворотів під час їзди
* Автоматичне включення стопсигналу під час гальмування та зупинки
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
  * Лінійність сервоприводу
* Налаштування мотора
  * Мінімальне значення ШИМ для рушання з місця
  * Лінійність газу
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
  * Драйвер CH340 з офіційного сайту можна скачати тут https://docs.wemos.cc/en/latest/ch340_driver.html
3. Заходете у диспечер пристроїів і перевіряєти чи всі драйвера встанорвлено і ваша плата розпізнається системою. 
  * Відкриваємо панель керування комп'ютером. 
![Call device manager](/img/device-manager.png)
  * Переходимо на пункт "Диспетчер пристроїв".
Ймовірно ваша плата буде називатися 'USB-Serial CH340 (COM_)'
![Call device manager](/img/usb-serial-ch340.png)
  * Запамятовуєте який номер порта отримала ваша плата (у мому випадку №3)
![Call device manager](/img/usb-serial-ch340-com3.png)
4. запускаєте Tools/1 upload.bat
5. Після старту - скрипт запитає номер порта до якого підєднано вашу плату
6. Вводите номер (тільки цифру), тиснете Enter
7. Чекаєте поки завершиться процеc завантаження

Все плата прошита.

З цього моменту нею можна користуватись.

Якщо ж ви бажаєте змінити деякі налаштування (діапазон повороту сервоприводу, стартову швидкість, яскравість світла... тощо), то додатково необхідно завантажити в память контроллера модуль налаштувань

## Завантаження модуля налаштувань
На цьому кроці ви вже маєте підключену до вашого USB плату і знаєте який номер порта вона отримала.
Приступаємо
1. запускаєте Tools/2 setup.bat
2. Після старту - скрипт запитає номер порта до якого підєднано вашу плату
3. Вводите номер (тільки цифру), тиснете Enter
4. Чекаєте поки завершиться процеc завантаження

Все плата поновлена.

## Налаштування
Для налаштування вам необхідно на телефоні підєднатися до wifi мережі Wemos_00000000. (замість нулів буде серійний номер вашої плати)
Стандартний пароль 12345678. Ви можете його змінити за вашим бажанням.

Після підключення - відкриваєте web переглядач і переходете на адресу 192.168.4.1/ Це адреса для налаштувань.

### Налаштування точки доступу
![Config](/img/config.png)
* SSID - назва вашої моделі у WIFI мережі. Це може бути наприклад номерний знак, або ваш нікнейм...
* PASSWORD - пароль доступу. Типово встановлено 12345678

### Налаштування сервоприводу
![Servo](/img/servo.png)
* center - Положення сервопривду при русі прямо, в градусах
* left - Положення сервопривду при вивороті коліс до упору в ліво, в градусах
* right - Положення сервопривду при вивороті коліс до упору в право, в градусах
* Stearing potenciometer linearity - лінійність руля
  * Linear - руль лінійний. Відхилення руля на 1 градус повертає колеса на 1 градус.
  * Y = X^2/X руль не лінійний. При позиціях близьких до нуля на один градус зміни положення руля колеса повертаються менше.
При позиціях близьких до крайніх положень - колеса повертають швидко. На високих швидкостях це дозволяє маневрувати плавнішше.

### Налаштування тягового мотора
![Engine](/img/engine.png)
* Minimum PWM speed - мінімальне значення ШИМ, яке необхідне для того, щоб мотор міг зрушити модель з місця.
* Speed potenciometer linearity - лінійність значень потенціометра.
  * Linear - потенціометр лінійний.
  * Y = X^2/X Потенціометр не лінійний. При позиціях близьких до нуля на одиницю зміни положення потенціометра швидкусть наростає повільно.
При позиціях близьких до макс положень - швидкість наростає швидко.

### Налаштування світла
![Light](/img/light.png)
* Head light PWM - значення ШИМ для переднього світла фар
* High light PWM - значення ШИМ для дальнього світла фар
* Parking light PWM - значення ШИМ для габаритних стоянкових вогнів.
* Turn light PWM - значення ШИМ для повороьів та аварійної світлової сигналізації.
* Stop light duration - проміжок часу на який включається  стопсигнал після зупинки моделі.
* Back light timeout - проміжок часу через який вимикається світло заднього ходу після зупинки моделі
* Back light PWM - Значення ШИМ для світла заднього ходу


## Підключення

1) Сервопривід Керуючий вивід => D5
2) Драйвер тягового мотора Вхід А => D7, Вхід Б => D6
3) Головне світло D4
4) Лівий поврот D2
5) Правий поворот D1
6) Задній хід D3
7) Стопсигнал D8
8) Габарит D0
9) ~*Бузер RX*~

## Спосіб підключення
![Wiring diagram](/img/schematic.png)
