# Терминальный сервер для Hive OS
## Требования:
Чистый Ubuntu Server 16.04 со статическим локальным IP-адресом.

Дисковое пространство: 10 Gb + ~450 Mb на каждый подключаемый риг.

Проводное сетевое подключение (желательно Gigabit Ethernet).

Оперативная память на бездисковых ригах: не менее 4-х Gb

## Краткие пояснения
При первом подключении (а также после обновления Hive OS) и загрузке рига, через локальную сеть передается значительный объем данных (около 400 Mb). Поэтому следует избегать одновременной загрузки большого количества новых бездисковых ригов. При последующих загрузках объем передаваемых данных значительно меньше (около 50 Mb).

Сервер может работать как на физическом железе, так и на виртуальной машине (проверено на VirtualBox, на других виртуалках не тестировалось). При установке на виртуальную машину сетевой интерфейс должен быть сконфигурирован как сетевой мост.

В процессе работы установочного скрипта, будет предложено скачать образ Hive OS. В случае, если у вас уже есть образ, можно отказаться от скачивания, и после завершения работы скрипта, разместить образ в развернутом виде (не в архиве) в директории /hiveserver/image

Данная сборка тестировалась на образе hive-0.5-76. На других версиях образа сборка может работать некорректно.

После завершения работы установочного скипта, настоятельно рекомендуется перезагрузить сервер, чтобы все изменения корректно вступили в силу.



## Важное замечание по поводу DHCP 
Возможны два варианта сетевых конфигураций:
1. Данный сервер будет выполнять функции сервера DHCP
2. В локальной сети уже имеется работающий DHCP сервер (например роутер)

Скрипт первоначальной установки предполагает оба варианта (будет задан соответствующий вопрос).
В случае, если в сети уже имеется DHCP сервер, во время установки необходимо отказаться от конфигурирования DHCP-сервера. Но при этом, на существующем DHCP сервере необходимо будет сконфигурировать две стандартные опции DHCP:

*66 (tftp-server, в некоторых реализациях next-server)* - должна иметь значение ip-адреса данного сервера

*67 (bootfile-name)* - должна иметь значение *"/pxelinux.0"*

установка данных опций поддерживаются не всеми моделями роутеров. В таком случае необходимо отключить на роутере функции DHCP-сервера. И сконфигурировать данный сервер в качестве DHCP-сервера
 
 
 
 ## Установка сервера
 
 ```
cd /tmp && wget https://raw.githubusercontent.com/lexandr0s/test_diskless/master/sbin/hiveserver-setup && sudo bash hiveserver-setup
 ```
 
## Загрузка бездисковых ригов
После установки и перезагрузки, сервер готов к работе и ожидает подключений.
Для загрузки бездисковых ригов по сети, необходимо включить соответствующую функцию в BIOS рига (net-boot, pxe-boot и тому подобное).
После чего, риг будет загружаться по локальной сети с данного сервера.
