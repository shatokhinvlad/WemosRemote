# Wemos Remote

Wemos remote - проект для керування авто моделлю з додатку на телефоні. 

Проект зроблено спеціально для моделеі ЗАЗ965А з Youtube каналу Евгения Былеева https://www.youtube.com/channel/UC27d-7p-ovLAEvgUijC3Dwg

![ЗАЗ965А](/img/zaz3.png)




Усі етапи переобки моделі:
* Частина 1 https://www.youtube.com/watch?v=Rnvk8hbOn04
* Частина 2 https://www.youtube.com/watch?v=U9sRm0HNj2A
* Частина 3 https://www.youtube.com/watch?v=dFrtj1GCY1U
* Частина 4 https://www.youtube.com/watch?v=j8VWTGg8k7Q

Керування реалізовано на процесорі ESP8266. Цей процесор компактний, має вбудований WIFI модуль і адаповане ядро під фреймворк Arduino. Прошивка реалізована у середовищі VisualStudio 2019 з використанням фреймворків:
* Arduino https://www.arduino.cc/en/Main/Software
* RemoteXY http://remotexy.com/ https://github.com/RemoteXY/RemoteXY-Arduino-library
* ESP8266 https://github.com/esp8266/Arduino
* jQuery https://jquery.com/
* Bootstrap https://getbootstrap.com/
* ESPAudio https://github.com/earlephilhower/ESP8266Audio

При необхідлності скетч можна редагувати у середовищі Arduino IDE.


# Основні можливості
## Можливості прошивки
* Керування через WIFI з телефона під управленням Android/IOs (з допомогою бібліотеки RemotXY http://remotexy.com). Проект інтерфейсу додатку можна знайти тут http://remotexy.com/en/editor/cc18fb05ab910a0eca09c8349ac4d7c3/
* Керування одним двигуном постійного струму
* Керування одним сервомотором
* Імітація постановки/зняття з сигналізації під час підєднання/відєднання додатку телефона
* Імітація клацання реле поворотів та пікання брелка сигналізації
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
  * Яскравість дальнього світла
  * Яскравість габаритних стоянкоянкових вогнів
  * Яскравість поворотників
  * Тривалість включення стопсигналу після зупинки моделі
  * Затримка вимкнення ліхтаря заднього хзоду після зупинки
  * Яскравість ліхтаря заднього хзод


# Мені вже все подобається! З чого розпочати?
## Що необхідно?
Зробити радіокерування для моделі дуже просто.

Базовий набір:
* Завантажена з GitHub копія проекту
* Модуль, або відлагодочна плата на базі процесора ESP8266 (Наприклад ESP12, ESP12-S, [Wemo D1 mini](https://wiki.wemos.cc/products:retired:d1_mini_v2.2.0), або інша.)
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
  * Драйвер CH340 з офіційного сайту можна скачати тут https://wiki.wemos.cc/downloads
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
9) Бузер RX

## Спосіб підключення (drv8833)

https://github.com/KushlaVR/WemosRemote/blob/ZAZ965A/img/schematic.SVG




<svg width="383.6mm" height="256.6mm"
 viewBox="0 0 7672 5132"
 xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"  version="1.2" baseProfile="tiny">
<title>Proteus SVG export</title>
<desc>Created by Proteus Design Suite</desc>
<defs>
</defs>
<g fill="none" stroke="black" stroke-width="1" fill-rule="evenodd" stroke-linecap="square" stroke-linejoin="bevel" >

<g fill="#ffffff" fill-opacity="1" stroke="none" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="non-scaling-stroke" fill-rule="evenodd" d="M0,0 L7672,0 L7672,5132 L0,5132 L0,0"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="10" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M26,26 L7646,26 L7646,5106 L26,5106 L26,26"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="10" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M128,128 L7544,128 L7544,5004 L128,5004 L128,128"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="869,128 869,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,128 128,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1611,128 1611,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="869,128 869,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2353,128 2353,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1611,128 1611,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3094,128 3094,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2353,128 2353,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3836,128 3836,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3094,128 3094,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4578,128 4578,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3836,128 3836,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5319,128 5319,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4578,128 4578,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6061,128 6061,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5319,128 5319,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6803,128 6803,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6061,128 6061,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7544,128 7544,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6803,128 6803,26 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="869,5106 869,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1611,5106 1611,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="869,5106 869,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2353,5106 2353,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1611,5106 1611,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3094,5106 3094,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2353,5106 2353,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3836,5106 3836,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3094,5106 3094,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4578,5106 4578,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3836,5106 3836,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5319,5106 5319,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4578,5106 4578,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6061,5106 6061,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5319,5106 5319,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6803,5106 6803,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6061,5106 6061,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7544,5106 7544,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6803,5106 6803,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,128 26,128 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,615 26,615 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,615 26,615 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,1103 26,1103 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,1103 26,1103 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,1591 26,1591 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,1591 26,1591 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,2078 26,2078 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,2078 26,2078 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,2566 26,2566 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,2566 26,2566 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,3054 26,3054 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,3054 26,3054 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,3541 26,3541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,3541 26,3541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,4029 26,4029 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,4029 26,4029 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,4517 26,4517 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,4517 26,4517 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,5004 26,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,128 7544,128 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,615 7544,615 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,615 7544,615 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,1103 7544,1103 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,1103 7544,1103 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,1591 7544,1591 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,1591 7544,1591 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,2078 7544,2078 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,2078 7544,2078 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,2566 7544,2566 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,2566 7544,2566 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,3054 7544,3054 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,3054 7544,3054 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,3541 7544,3541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,3541 7544,3541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,4029 7544,4029 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,4029 7544,4029 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,4517 7544,4517 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,4517 7544,4517 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7646,5004 7544,5004 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="128,5004 128,5106 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="480" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1222" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >B</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1964" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2705" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >D</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3448" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >E</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4191" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >F</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4929" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >G</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5672" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >H</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6418" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >J</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7156" y="89" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >K</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1217" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >B</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1959" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2700" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >D</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3443" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >E</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4186" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >F</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4923" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >G</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5667" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >H</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6413" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >J</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7150" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >K</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="475" y="5072" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5766,4580 L7544,4580 L7544,5002 L5766,5002 L5766,4580"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5792" y="4648" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >FILE NAME:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5792" y="4963" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >BY:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7077" y="4638" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >DATE:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7082" y="4826" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >PAGE:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6107" y="4652" font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
 >schematic.pdsprj</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7077" y="4733" font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
 >07.02.2020</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5904" y="4963" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >@AUTHOR</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="7047,4580 7047,5002 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6107" y="4866" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >C:\Users\KushlaVR\Desktop\schematic.pdsprj</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5792" y="4863" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >PATH:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7074" y="4887" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7166" y="4887" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >of</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7277" y="4887" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6708" y="4965" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >REV:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6807" y="4968" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >@REV</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7081" y="4965" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >TIME:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7258" y="4965" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >15:59:59</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5786" y="4757" font-family="Arial" font-size="40.64" font-weight="700" font-style="normal" 
 >DESIGN TITLE:</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6107" y="4767" font-family="Arial" font-size="69.088" font-weight="700" font-style="normal" 
 >schematic.pdsprj</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="388" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="876" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="1364" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="2339" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="2827" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >5</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="3315" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >6</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="3802" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >7</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="4290" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >8</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="4778" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >9</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="388" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="876" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="1369" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="1851" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="2339" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="2827" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >5</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="3315" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >6</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="3802" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >7</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="4290" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >8</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="64" y="1851" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="7582" y="4778" font-family="Arial" font-size="48.768" font-weight="700" font-style="normal" 
 >9</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="9" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,636 L4141,636"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,686 L3607,686 L3607,610 L3531,610"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,737 L3531,737"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,788 L3607,788 L3607,940 L3531,940"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,839 L3658,839 L3658,991 L3531,991"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,890 L3709,890 L3709,1042 L3531,1042"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,940 L3760,940 L3760,1093 L3531,1093"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,991 L4141,991"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,890 L5817,890 L5817,813 L5919,813"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,839 L5766,839 L5766,763 L5919,763"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,788 L5665,788 L5665,661 L5919,661"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,737 L5614,737 L5614,610 L5919,610"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,686 L5512,686 L5512,458 L5919,458"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,636 L5157,636"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5792,1220 L5538,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,991 L5538,991 L5538,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5538,1296 L5538,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6122,1296 L6122,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5081,940 L6122,940 L6122,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5944,1220 L6122,1220"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4471,3760 L4369,3760 L4369,3988"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4369,4700 L4369,4598"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4369,4293 L4369,4598"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,4522 L4065,4598 L4369,4598"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,4268 L4065,4141"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4217,4141 L4065,4141"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3988,4141 L4065,4141"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,4141 L4065,3988"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4369,3582 L4369,3658"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4471,3658 L4369,3658"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,3734 L4065,3658 L4369,3658"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,4598 L940,4674"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1398,4598 L1398,4674"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2109,4598 L2109,4674"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,4598 L2668,4674"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,2261 L2668,2312"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2464,2261 L2464,2312"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1855,2261 L1855,2312"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1194,2261 L1194,2312"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,2261 L940,2312"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1855,2058 L1855,1906 L1652,1906 L1652,3480 L3099,3480"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,2058 L940,1906 L661,1906 L661,3074"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,4395 L940,4090 L661,4090 L661,3074"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3099,3074 L661,3074"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,2058 L2668,1956 L3049,1956 L3049,3176"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,4395 L2668,4293 L3049,4293 L3049,3176"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3099,3176 L3049,3176"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1194,2058 L1194,1753 L2109,1753"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2464,2058 L2464,1753 L2109,1753"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3099,3277 L2109,3277 L2109,1753"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1398,4395 L1398,4090 L2109,4090"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2109,4395 L2109,4090"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3099,3379 L2109,3379 L2109,4090"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4598,1931 L4598,1906 L4700,1906 L4700,1931"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3963,2287 L3658,2287 L3658,1372 L2769,1372 L2769,991 L3074,991"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3074,1042 L2820,1042 L2820,1423 L3607,1423 L3607,2337 L3963,2337"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2922,1093 L2871,1093 L2871,1474 L3557,1474 L3557,2490 L3963,2490"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5284,3150 L4649,3150 L4649,2871 L3506,2871 L3506,1525 L2718,1525 L2718,940 L2972,940"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5157,2439 L5360,2439 L5360,2515 L5436,2515"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5157,2490 L5309,2490 L5309,2693 L5436,2693"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5157,2388 L5360,2388 L5360,1880 L5462,1880"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1880,1118 L1880,1169"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1880,483 L1880,432"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1880,864 L1880,788"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1880,737 L1880,788"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3023,610 L2312,610 L2312,788 L1880,788"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3963,2439 L3861,2439 L3861,2617"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3963,2388 L3861,2388 L3861,2134"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5843,1144 5843,1296 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5868,1194 5868,1245 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5868,1220 5893,1220 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5792,1220 5843,1220 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5944,1220 5893,1220 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5839" y="1128" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >LI-ION</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5839" y="1338" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >4.2V</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4319,458 L4979,458 L4979,1093 L4319,1093 L4319,458"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4471,585 4471,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4471,483 4573,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4573,483 4573,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4573,585 4649,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4649,585 4649,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4649,483 4725,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4725,483 4725,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4725,585 4776,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4776,585 4776,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4471,610 L4827,610 L4827,991 L4471,991 L4471,610"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4573,1017 L4725,1017 L4725,1118 L4573,1118 L4573,1017"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4776,483 4827,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4827,483 4827,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4522,610 4522,483 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,636 4319,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="650" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RST</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4182" y="628" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RST</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,686 4319,686 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="700" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4217" y="678" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,737 4319,737 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="751" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4214" y="729" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,788 4319,788 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="802" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D5</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4214" y="780" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D5</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,839 4319,839 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="853" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D6</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4214" y="831" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D6</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,890 4319,890 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="904" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D7</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4214" y="882" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D7</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,940 4319,940 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="954" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D8</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4214" y="932" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D8</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,991 4319,991 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4344" y="1005" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >3V3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4194" y="983" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >3V3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,636 4979,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4899" y="650" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >TX</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="628" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >TX</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,686 4979,686 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4895" y="700" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RX</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="678" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RX</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,737 4979,737 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4900" y="751" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="729" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,788 4979,788 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4900" y="802" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="780" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,839 4979,839 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4900" y="853" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="831" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,890 4979,890 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4900" y="904" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="882" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >D4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,940 4979,940 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4859" y="954" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >GND</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="932" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >GND</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5081,991 4979,991 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4903" y="1005" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >5V</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5030" y="983" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >5V</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4315" y="442" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >WEMOS D1 MINI</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4315" y="1160" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >WEMOS D1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4090,656 L4090,615 L4039,636 L4090,656"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4090,636 4141,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3872" y="650" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RESET</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,631 L3480,590 L3430,610 L3480,631"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,610 3531,610 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3130" y="624" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >ANALOG_3V3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,758 L3480,717 L3430,737 L3480,758"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,737 3531,737 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3148" y="751" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >STOP-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,961 L3480,920 L3430,940 L3480,961"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,940 3531,940 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3084" y="954" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SERVO-MOTOR</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,1012 L3480,971 L3430,991 L3480,1012"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,991 3531,991 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3204" y="1005" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >MOTOR-A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,1062 L3480,1022 L3430,1042 L3480,1062"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,1042 3531,1042 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3204" y="1056" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >MOTOR-B</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3480,1113 L3480,1072 L3430,1093 L3480,1113"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3480,1093 3531,1093 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3065" y="1107" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >WIPERS-MOTOR</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4090,1012 L4090,971 L4039,991 L4090,1012"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4090,991 4141,991 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3916" y="1005" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+3V3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5558,1347 L5517,1347 L5538,1398 L5558,1347"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5538,1347 5538,1296 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,4031,7073)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5552" y="1521" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+5V0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5970,793 L5970,834 L6020,813 L5970,793"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5970,813 5919,813 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="827" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >FRON-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5970,742 L5970,783 L6020,763 L5970,742"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5970,763 5919,763 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="777" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SIGN-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5970,641 L5970,681 L6020,661 L5970,641"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5970,661 5919,661 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="675" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >LEFT-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5970,590 L5970,631 L6020,610 L5970,590"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5970,610 5919,610 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="624" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RIGHT-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5970,437 L5970,478 L6020,458 L5970,437"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5970,458 5919,458 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="472" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SOUND</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5208,615 L5208,656 L5258,636 L5208,615"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5208,636 5157,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5284" y="650" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >TXD</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6122,1296 6122,1347 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6071,1347 6173,1347 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6097,1372 6147,1372 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6117,1398 6127,1398 " />
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5548,1220 C5548,1225.52 5543.52,1230 5538,1230 C5532.48,1230 5528,1225.52 5528,1220 C5528,1214.48 5532.48,1210 5538,1210 C5543.52,1210 5548,1214.48 5548,1220 "/>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6132,1220 C6132,1225.52 6127.52,1230 6122,1230 C6116.48,1230 6112,1225.52 6112,1220 C6112,1214.48 6116.48,1210 6122,1210 C6127.52,1210 6132,1214.48 6132,1220 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4598,3633 C4557,3633 4522,3668 4522,3709 C4522,3750 4557,3785 4598,3785 L4598,3633"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4522,3760 4542,3760 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4522,3658 4542,3658 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4471,3760 4522,3760 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4471,3658 4522,3658 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4518" y="3617" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >BUZ1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4518" y="3827" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >BUZZER</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4369,4242 L4319,4217 L4344,4192 L4369,4242"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4268,4065 L4293,4065 L4293,4217 L4268,4217 L4268,4065"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4293,4115 4369,4039 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4293,4166 4329,4202 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4217,4141 4268,4141 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4369,3988 4369,4039 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4369,4293 4369,4242 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4399" y="4090" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >Q1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4399" y="4149" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >NPN</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4369,4700 4369,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4319,4750 4420,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4344,4776 4395,4776 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4364,4801 4374,4801 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4044,4319 L4085,4319 L4085,4471 L4044,4471 L4044,4319"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4065,4522 4065,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4065,4268 4065,4319 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4114" y="4370" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >R1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4114" y="4429" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >1k</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4379,4598 C4379,4603.52 4374.52,4608 4369,4608 C4363.48,4608 4359,4603.52 4359,4598 C4359,4592.48 4363.48,4588 4369,4588 C4374.52,4588 4379,4592.48 4379,4598 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3938,4161 L3938,4120 L3887,4141 L3938,4161"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3938,4141 3988,4141 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3707" y="4155" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SOUND</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4075,4141 C4075,4146.52 4070.52,4151 4065,4151 C4059.48,4151 4055,4146.52 4055,4141 C4055,4135.48 4059.48,4131 4065,4131 C4070.52,4131 4075,4135.48 4075,4141 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4349,3531 L4390,3531 L4369,3480 L4349,3531"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4369,3531 4369,3582 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,928,7838)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4383" y="3455" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+5V0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4044,3785 L4085,3785 L4085,3938 L4044,3938 L4044,3785"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4065,3988 4065,3938 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4065,3734 4065,3785 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4114" y="3836" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >R2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4114" y="3895" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >10k</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4379,3658 C4379,3663.52 4374.52,3668 4369,3668 C4363.48,3668 4359,3663.52 4359,3658 C4359,3652.48 4363.48,3648 4369,3648 C4374.52,3648 4379,3652.48 4379,3658 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1001,2160 C1001,2193.69 973.689,2221 940,2221 C906.311,2221 879,2193.69 879,2160 C879,2126.31 906.311,2099 940,2099 C973.689,2099 1001,2126.31 1001,2160 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="966,2185 915,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,2109 940,2144 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,2185 940,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,2185 L910,2144 L971,2144 L940,2185"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,2058 940,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-1080,3036)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="978" y="2058" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,2261 940,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-1314,3270)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="978" y="2292" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1255,2160 C1255,2193.69 1227.69,2221 1194,2221 C1160.31,2221 1133,2193.69 1133,2160 C1133,2126.31 1160.31,2099 1194,2099 C1227.69,2099 1255,2126.31 1255,2160 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1220,2185 1169,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1194,2109 1194,2144 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1194,2185 1194,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1194,2185 L1164,2144 L1225,2144 L1194,2185"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1194,2058 1194,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-826,3290)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1232" y="2058" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1194,2261 1194,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-1060,3524)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1232" y="2292" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2729,2160 C2729,2193.69 2701.69,2221 2668,2221 C2634.31,2221 2607,2193.69 2607,2160 C2607,2126.31 2634.31,2099 2668,2099 C2701.69,2099 2729,2126.31 2729,2160 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2693,2185 2642,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,2109 2668,2144 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,2185 2668,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,2185 L2637,2144 L2698,2144 L2668,2185"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,2058 2668,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,648,4764)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2706" y="2058" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,2261 2668,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,414,4998)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2706" y="2292" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2525,2160 C2525,2193.69 2497.69,2221 2464,2221 C2430.31,2221 2403,2193.69 2403,2160 C2403,2126.31 2430.31,2099 2464,2099 C2497.69,2099 2525,2126.31 2525,2160 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2490,2185 2439,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2464,2109 2464,2144 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2464,2185 2464,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2464,2185 L2434,2144 L2495,2144 L2464,2185"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2464,2058 2464,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,444,4560)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2502" y="2058" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2464,2261 2464,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,210,4794)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2502" y="2292" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1916,2160 C1916,2193.69 1888.69,2221 1855,2221 C1821.31,2221 1794,2193.69 1794,2160 C1794,2126.31 1821.31,2099 1855,2099 C1888.69,2099 1916,2126.31 1916,2160 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,2185 1829,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,2109 1855,2144 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,2185 1855,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1855,2185 L1824,2144 L1885,2144 L1855,2185"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,2058 1855,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-165,3951)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1893" y="2058" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,2261 1855,2210 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-399,4185)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1893" y="2292" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1001,4496 C1001,4529.69 973.689,4557 940,4557 C906.311,4557 879,4529.69 879,4496 C879,4462.31 906.311,4435 940,4435 C973.689,4435 1001,4462.31 1001,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="966,4522 915,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,4446 940,4481 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,4522 940,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M940,4522 L910,4481 L971,4481 L940,4522"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,4395 940,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-3417,5373)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="978" y="4395" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,4598 940,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-3651,5607)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="978" y="4629" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1459,4496 C1459,4529.69 1431.69,4557 1398,4557 C1364.31,4557 1337,4529.69 1337,4496 C1337,4462.31 1364.31,4435 1398,4435 C1431.69,4435 1459,4462.31 1459,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1423,4522 1372,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1398,4446 1398,4481 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1398,4522 1398,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1398,4522 L1367,4481 L1428,4481 L1398,4522"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1398,4395 1398,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-2959,5831)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1436" y="4395" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1398,4598 1398,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-3193,6065)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1436" y="4629" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2729,4496 C2729,4529.69 2701.69,4557 2668,4557 C2634.31,4557 2607,4529.69 2607,4496 C2607,4462.31 2634.31,4435 2668,4435 C2701.69,4435 2729,4462.31 2729,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2693,4522 2642,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,4446 2668,4481 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,4522 2668,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2668,4522 L2637,4481 L2698,4481 L2668,4522"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,4395 2668,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-1689,7101)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2706" y="4395" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,4598 2668,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-1923,7335)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2706" y="4629" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2170,4496 C2170,4529.69 2142.69,4557 2109,4557 C2075.31,4557 2048,4529.69 2048,4496 C2048,4462.31 2075.31,4435 2109,4435 C2142.69,4435 2170,4462.31 2170,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2134,4522 2083,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2109,4446 2109,4481 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2109,4522 2109,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2109,4522 L2078,4481 L2139,4481 L2109,4522"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2109,4395 2109,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-2248,6542)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2147" y="4395" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2109,4598 2109,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,-2482,6776)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2147" y="4629" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >C</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,4674 940,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="890,4725 991,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="915,4750 966,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="935,4776 945,4776 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1398,4674 1398,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1347,4725 1448,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1372,4750 1423,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1393,4776 1403,4776 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2109,4674 2109,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2058,4725 2160,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2083,4750 2134,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2104,4776 2114,4776 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,4674 2668,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2617,4725 2718,4725 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2642,4750 2693,4750 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2663,4776 2673,4776 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2668,2312 2668,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2617,2363 2718,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2642,2388 2693,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2663,2414 2673,2414 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2464,2312 2464,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2414,2363 2515,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2439,2388 2490,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2459,2414 2469,2414 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,2312 1855,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1804,2363 1906,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1829,2388 1880,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1850,2414 1860,2414 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1194,2312 1194,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1144,2363 1245,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1169,2388 1220,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1189,2414 1199,2414 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="940,2312 940,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="890,2363 991,2363 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="915,2388 966,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="935,2414 945,2414 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3150,3054 L3150,3094 L3201,3074 L3150,3054"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3150,3074 3099,3074 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3226" y="3088" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >LEFT-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3150,3155 L3150,3196 L3201,3176 L3150,3155"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3150,3176 3099,3176 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3226" y="3190" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RIGHT-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3150,3257 L3150,3298 L3201,3277 L3150,3257"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3150,3277 3099,3277 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3226" y="3291" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >STOP-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3150,3358 L3150,3399 L3201,3379 L3150,3358"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3150,3379 3099,3379 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3226" y="3393" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >FRON-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3150,3460 L3150,3501 L3201,3480 L3150,3460"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3150,3480 3099,3480 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3226" y="3494" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SIGN-LIGHT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M671,3074 C671,3079.52 666.523,3084 661,3084 C655.477,3084 651,3079.52 651,3074 C651,3068.48 655.477,3064 661,3064 C666.523,3064 671,3068.48 671,3074 "/>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3059,3176 C3059,3181.52 3054.52,3186 3049,3186 C3043.48,3186 3039,3181.52 3039,3176 C3039,3170.48 3043.48,3166 3049,3166 C3054.52,3166 3059,3170.48 3059,3176 "/>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2119,1753 C2119,1758.52 2114.52,1763 2109,1763 C2103.48,1763 2099,1758.52 2099,1753 C2099,1747.48 2103.48,1743 2109,1743 C2114.52,1743 2119,1747.48 2119,1753 "/>
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2119,4090 C2119,4095.52 2114.52,4100 2109,4100 C2103.48,4100 2099,4095.52 2099,4090 C2099,4084.48 2103.48,4080 2109,4080 C2114.52,4080 2119,4084.48 2119,4090 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5868,3074 L6478,3074 L6478,3760 L5868,3760 L5868,3074"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6478,3125 L6706,3125 L6706,3455 L6478,3455 L6478,3125"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6706,3226 L6757,3226 L6757,3379 L6706,3379 L6706,3226"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6757,3150 L6808,3150 L6808,3684 L6757,3684 L6757,3150"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5817,3171 L5817,3130 L5766,3150 L5817,3171"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,3150 5868,3150 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5421" y="3164" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SERVO-MOTOR</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5817,3298 L5817,3257 L5766,3277 L5817,3298"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,3277 5868,3277 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5643" y="3291" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+5V0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5893" y="3417" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >BROWN</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5893" y="3290" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >RED</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5893" y="3163" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >YELLOW</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5868,3404 5817,3404 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,3353 5817,3455 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5792,3379 5792,3430 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5766,3399 5766,3409 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5868,2363 L6503,2363 L6503,2820 L5868,2820 L5868,2363"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6503,2388 L6579,2388 L6579,2795 L6503,2795 L6503,2388"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6579,2566 L6960,2566 L6960,2617 L6579,2617 L6579,2566"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5817,2536 L5817,2495 L5766,2515 L5817,2536"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,2515 5868,2515 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5546" y="2529" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >MOTOR A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5817,2713 L5817,2673 L5766,2693 L5817,2713"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,2693 5868,2693 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5544" y="2707" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >MOTOR B</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5868,1728 L6503,1728 L6503,2185 L5868,2185 L5868,1728"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6503,1753 L6579,1753 L6579,2160 L6503,2160 L6503,1753"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6579,1931 L6960,1931 L6960,1982 L6579,1982 L6579,1931"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5817,1901 L5817,1860 L5766,1880 L5817,1901"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,1880 5868,1880 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="5565" y="1894" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >WIPER A</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5868,2083 5817,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5817,2033 5817,2134 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5792,2058 5792,2109 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5766,2078 5766,2088 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,4522 852,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,4471 801,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,4471 801,4420 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,4522 801,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,4522 801,4573 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,4573 724,4496 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="724,4496 801,4420 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,2185 852,2134 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,2134 801,2134 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,2134 801,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="852,2185 801,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,2185 801,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="801,2236 724,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="724,2160 801,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,2134 2756,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,2185 2807,2185 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,2185 2807,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,2134 2807,2134 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,2134 2807,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,2083 2884,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2884,2160 2807,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,4471 2756,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,4522 2807,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,4522 2807,4573 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2756,4471 2807,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,4471 2807,4420 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2807,4420 2884,4496 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2884,4496 2807,4573 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1207,4573 C1264,4573 1309,4539 1309,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1309,4496 C1309,4454 1264,4420 1207,4420 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1207,4420 C1193,4420 1182,4454 1182,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1182,4496 C1182,4539 1193,4573 1207,4573 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1156,4446 1080,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1156,4471 1080,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1156,4496 1080,4496 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1156,4522 1080,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1156,4547 1080,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2299,4420 C2242,4420 2198,4454 2198,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2198,4496 C2198,4539 2242,4573 2299,4573 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2299,4573 C2314,4573 2325,4539 2325,4496 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M2325,4496 C2325,4454 2314,4420 2299,4420 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2350,4547 2426,4547 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2350,4522 2426,4522 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2350,4496 2426,4496 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2350,4471 2426,4471 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2350,4446 2426,4446 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1410,2083 1334,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1283,2160 1334,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1410,2083 1461,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1461,2160 1410,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1410,2236 1334,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1334,2236 1283,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1309" y="2174" font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
 >STOP</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="3" stroke-linecap="butt" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1309,2176 1439,2176 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2325,2083 2248,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2198,2160 2248,2083 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2325,2083 2376,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2376,2160 2325,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2325,2236 2248,2236 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2248,2236 2198,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="2223" y="2174" font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
 >STOP</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="3" stroke-linecap="butt" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="44.704" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2223,2176 2353,2176 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1791" y="2490" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >SIGN</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1715,2452 1994,2452 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1994,2452 1994,2528 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1994,2528 1715,2528 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1715,2528 1715,2452 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4293,2160 L4801,2160 L4801,2668 L4293,2668 L4293,2160"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="52.832" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4446" y="2258" font-family="Arial" font-size="52.832" font-weight="400" font-style="normal" 
 >DRV8833</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2307 L4242,2266 L4192,2287 L4242,2307"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2287 4293,2287 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4101" y="2301" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >IN1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2358 L4242,2317 L4192,2337 L4242,2358"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2337 4293,2337 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4101" y="2351" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >IN2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2409 L4242,2368 L4192,2388 L4242,2409"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2388 4293,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4076" y="2402" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >VCC</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2459 L4242,2419 L4192,2439 L4242,2459"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2439 4293,2439 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4071" y="2453" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >GND</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2510 L4242,2469 L4192,2490 L4242,2510"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2490 4293,2490 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4101" y="2504" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >IN3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4242,2561 L4242,2520 L4192,2541 L4242,2561"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4242,2541 4293,2541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4101" y="2555" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >IN4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2520 L4852,2561 L4903,2541 L4852,2520"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2541 4801,2541 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2555" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >EEP</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2469 L4852,2510 L4903,2490 L4852,2469"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2490 4801,2490 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2504" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >OUT1</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2419 L4852,2459 L4903,2439 L4852,2419"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2439 4801,2439 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2453" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >OUT2</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2368 L4852,2409 L4903,2388 L4852,2368"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2388 4801,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2402" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >OUT3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2317 L4852,2358 L4903,2337 L4852,2317"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2337 4801,2337 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2351" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >OUT4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4852,2266 L4852,2307 L4903,2287 L4852,2266"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4852,2287 4801,2287 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="4928" y="2301" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >ULT</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4578,2109 L4618,2109 L4598,2058 L4578,2109"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4598,2109 4598,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4679,2109 L4720,2109 L4700,2058 L4679,2109"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4700,2109 4700,2160 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4578,2033 L4618,2033 L4598,1982 L4578,2033"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4598,1982 4598,1931 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4679,2033 L4720,2033 L4700,1982 L4679,2033"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4700,1982 4700,1931 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,2307 L4065,2266 L4014,2287 L4065,2307"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4014,2287 3963,2287 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3176,1012 L3176,971 L3125,991 L3176,1012"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3125,991 3074,991 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3176,1062 L3176,1022 L3125,1042 L3176,1062"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3125,1042 3074,1042 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,2358 L4065,2317 L4014,2337 L4065,2358"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4014,2337 3963,2337 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3023,1113 L3023,1072 L2972,1093 L3023,1113"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="2972,1093 2922,1093 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,2510 L4065,2469 L4014,2490 L4065,2510"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4014,2490 3963,2490 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5385,3171 L5385,3130 L5335,3150 L5385,3171"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5335,3150 5284,3150 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3074,961 L3074,920 L3023,940 L3074,961"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3023,940 2972,940 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5055,2469 L5055,2510 L5106,2490 L5055,2469"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5106,2490 5157,2490 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5055,2419 L5055,2459 L5106,2439 L5055,2419"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5106,2439 5157,2439 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5055,2368 L5055,2409 L5106,2388 L5055,2368"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5106,2388 5157,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5538,2536 L5538,2495 L5487,2515 L5538,2536"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5487,2515 5436,2515 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5538,2713 L5538,2673 L5487,2693 L5538,2713"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5487,2693 5436,2693 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M5563,1901 L5563,1860 L5512,1880 L5563,1901"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5512,1880 5462,1880 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1860,534 L1901,534 L1901,686 L1860,686 L1860,534"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,737 1880,686 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,483 1880,534 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1930" y="585" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >R3</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1930" y="644" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >10k</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1860,915 L1901,915 L1901,1067 L1860,1067 L1860,915"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,1118 1880,1067 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,864 1880,915 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1930" y="966" font-family="Arial" font-size="60.96" font-weight="400" font-style="normal" 
 >R4</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1930" y="1025" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >10k</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,1169 1880,1220 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1829,1220 1931,1220 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1855,1245 1906,1245 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1875,1271 1885,1271 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1860,382 L1901,382 L1880,331 L1860,382"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1880,382 1880,432 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,1589,2199)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="1894" y="305" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+5V0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3125,631 L3125,590 L3074,610 L3125,631"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3074,610 3023,610 " />
</g>

<g fill="#000000" fill-opacity="1" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M1890,788 C1890,793.523 1885.52,798 1880,798 C1874.48,798 1870,793.523 1870,788 C1870,782.477 1874.48,778 1880,778 C1885.52,778 1890,782.477 1890,788 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,2409 L4065,2368 L4014,2388 L4065,2409"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4014,2388 3963,2388 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M4065,2459 L4065,2419 L4014,2439 L4065,2459"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="4014,2439 3963,2439 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3861,2617 3861,2668 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3811,2668 3912,2668 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3836,2693 3887,2693 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3856,2718 3866,2718 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M3841,2083 L3882,2083 L3861,2033 L3841,2083"/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="3861,2083 3861,2134 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(0,-1,1,0,1868,5882)"
font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="3875" y="2007" font-family="Arial" font-size="40.64" font-weight="400" font-style="normal" 
 >+5V0</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="1" stroke-linecap="square" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="Arial" font-size="365.76" font-weight="400" font-style="normal" 
>
<text fill="#000000" fill-opacity="1" stroke="none" xml:space="preserve" x="6046" y="2694" font-family="Arial" font-size="365.76" font-weight="400" font-style="normal" 
 >M</text>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="5995,2058 6071,1931 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6071,1880 6071,2007 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6020,1855 C6120,1855 6198,1910 6198,2033 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6249,2058 6325,1931 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="6325,1880 6325,2007 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<path vector-effect="none" fill-rule="evenodd" d="M6274,1855 C6374,1855 6452,1910 6452,2033 "/>
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1601,966 1779,966 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1779,966 1779,610 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1779,610 1601,610 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1601,610 1601,966 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1652,610 1652,559 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1652,559 1728,559 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1728,559 1728,585 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,636 1753,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,636 1753,712 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,712 1626,712 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,712 1626,636 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,737 1753,737 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,737 1753,813 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,813 1626,813 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,813 1626,737 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,839 1753,839 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,839 1753,915 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1753,915 1626,915 " />
</g>

<g fill="none" stroke="#000000" stroke-opacity="1" stroke-width="8" stroke-linecap="round" stroke-linejoin="bevel" transform="matrix(1,0,0,1,0,0)"
font-family="MS Shell Dlg 2" font-size="58.2083" font-weight="400" font-style="normal" 
>
<polyline fill="none" vector-effect="none" points="1626,915 1626,839 " />
</g>
</g>
</svg>
