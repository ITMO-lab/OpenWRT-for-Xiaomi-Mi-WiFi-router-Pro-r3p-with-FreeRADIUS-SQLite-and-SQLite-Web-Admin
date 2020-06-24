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

[**2 Стоковая прошивка**](#2-стоковая-прошивка)





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

# 2 Стоковая прошивка

Если вы уже подключили роутер и настроили его на китайской прошивке, 

