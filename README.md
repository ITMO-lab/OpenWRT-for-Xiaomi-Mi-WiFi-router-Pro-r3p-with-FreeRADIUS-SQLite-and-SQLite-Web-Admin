# Установка OpenWRT на роутер Xiaomi R3P [ Xiaomi Mi WiFi router Pro(r3p) ] с настройкой WiFi wpa2-enterprise с запущеными на роутере FreeRADIUS (для предоставления индивидуальных ключей wpa2-enterprise), SQLite (для хранения этих ключей) и SQLite Web Admin (для удобного редактирования учётных записей пользователей)

# !!!!!!!!!!!!!!!!!!!!!!!

# !!! WARNING !!!

# !!!!!!!!!!!!!!!!!!!!!!!

# Прежде всего рекомендую ознакомиться с оглавлением, так как эту инструкцию могут читать разные люди: кто-то только купил роутер, кто-то уже удалил базовую прошивку, а кто-то уже "ОКИРПИЧИЛ" это чудо Китайского Роутеростроения :D	(без доли сарказма, классный быстрый роутер) 

# Автор постарался написать инструкцию таким образом, чтобы смог разобраться даже ещё больший ламер, чем автор. несмотря на это автор открыт к критике, исправлению и модификации инструкций. 

# Вы делаете все описанные ниже действия на свой страх и риск. Автор не несёт ответственности за возможный выход вашего роутера из строя, а так же за прекращение поддержки базовых пакетов роутера (отсылка к тем, кто пытался нормально настроить freeradius, который не имеет обратной совместимости даже среди версий одного релиза). У Автора не было перебоев электропитания непосредственно во время прошивки роутера (не во время установки пакетов), но даже если они или любые критические поломки базовых пакетов случатся у вас, не переживайте, всегда можно прошить ядро заново через UART.

# !!!!!!!!!!!!!!!!!!!!!!!

# !!! WARNING!!!

# !!!!!!!!!!!!!!!!!!!!!!!

# **ОГЛАВЛЕНИЕ**

[**1 Краткий обзор основных характеристик роутера**](#1-краткий-обзор-основных-характеристик-роутера)

[**2 Первая настройка роутера**](#2-первая-настройка-роутера)

[**3 Подготовка роутера к установке кастомных прошивок**](#3-подготовка-роутера-к-установке-кастомных-прошивок)



 

# 1 Краткий обзор основных характеристик роутера

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/photos/20200624_162317.jpg)

Характеристики роутера позволяют запускать на нём даже полноценные web сервисы:

| Параметр                                  | Значение                                 |
| ----------------------------------------- | ---------------------------------------- |
| Класс Wi-Fi                               | AC2600                                   |
| Максимальная скорость и стандарты 2.4 ГГц | 802.11 b/g/n,          800 Мбит/сек      |
| Максимальная скорость и стандарты 5 ГГц   | 802.11 AC/b/g/n,    1733 Мбит/сек        |
| Скорость LAN порта                        | 1000 Мбит/сек                            |
| Центральный процессор                     | MT7621A MIPS Dual Core 880MHz (4 потока) |
| Встроенная память                         | 256M SLC Nand Flash                      |
| Оперативная память                        | 512MB DDR3-1200                          |
| Процессор Wi-Fi                           | MediaTek MT7615E                         |

# 2 Первая настройка роутера

Если вы включаете роутер впервые, тогда он раздаст открытый WiFi с названием  Xiaomi_XXXX_XXXX

Подключаетесь к этому WiFi и переходите по адресу 192.168.31.1

Автоматически откроется страница http://192.168.31.1/cgi-bin/luci/web/init/hello

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/1.png)

Гугл переводчик переводит это как:
[текст] Пожалуйста, прочтите «Пользовательское лицензионное соглашение» и выберите, соглашаться ли 
[галочка] Присоединяйтесь к «Программе улучшения взаимодействия с пользователем»

*Лично я галочку снял*

Нажимаем большую синюю кнопку и попадаем в меню первой настройки роутера

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/2.png)

Или же, если в роутер не воткнут кабель с интернетом (перекинул на ноут, чтобы переводить китайский)

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/3.png)

Выбираем верхний режим работы и попадаем в меню выше

Логично, что тут выбирается имя сети WiFi (SSID) и её пароль, тут я галочку не снял (отвечает за какие-то настройки firewall. нам всё равно прошивку стирать, так что не важно) и нажал большую синюю кнопку.

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/4.png)

Тут выбирается пароль к админке этой прошивке. Если нажмёте галочку, тогда будет выбран тот же пароль, что и для WiFi. В чём разница между "Семья", "Компания" и "Подгоняет" я не понял, ну и не важно, пусть будет по умолчанию ("Семья"), нам ещё не долго пользоваться этой прошивкой. Снова нажимаем большую синюю кнопку. 

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/5.png)

Ждём загрузки роутера. На нём оранжевая лампочка сменится синей, когда всё завершится. Кстати, роутер ещё умеет мигать красным, если вы попытаетесь прошить его через USB такой прошивкой, которую он принципиально не может скушать.

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/6.png)

Думаю, тут очевидно, куда нажимать. Только не забудьте подключиться к новой Wi-Fi сети )

![img](https://github.com/ITMO-lab/OpenWRT-for-Xiaomi-Mi-WiFi-router-Pro-r3p-with-FreeRADIUS-SQLite-and-SQLite-Web-Admin/blob/images/screenshots/2/7.png)

Попадаем в такое меню. Здесь вводим пароль от роутера и нажимаем стрелочку. При первом входе роутер предложит провести некоторые настройки. Там есть синяя кнопка.     НЕ     НАЖИМАЙТЕ     НА     НЕЁ     . Вместо этого нажмите на подчёркнутый текст под кнопкой. Это пропустит гайд по этой прошивке. Прокликайте по свободным местам, чтобы пройти навязанное введение в интерфейс прошивки. Всё, с этого момента можно приступать к настройке роутера для установки кастомных прошивок.

# 3 Подготовка роутера к установке кастомных прошивок

*Часть инструкций в этой главе была взята с открытого источника:* https://openwrt.org/toh/xiaomi/xiaomi_r3p_pro 

