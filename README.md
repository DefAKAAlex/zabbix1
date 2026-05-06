# Zabbix 1

При выполнения задания использовались Виртуальные машины с Debian

Zabbiz1 - ВМ с сервером и агентом
Zabbix2 - только с агентом.

Ход дейстий соответсвовал приведённым в лекции.
Задания выполенны полностью.
Материал освоен.

## Задание 1
`Установите Zabbix Server с веб-интерфейсом.`

`Установите PostgreSQL.`
`Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.`


1) Изначально устанавливаем Postgresql на
```
root@zabbix1:~# apt install postgresql
```
Дальше используем подсказки, которые нам даёт сайт https://www.zabbix.com/

![PrefServ](https://github.com/DefAKAAlex/zabbix1/blob/main/img/pref_srv.png)

2) Устанавливаем репозиторий Zabbix
```
root@zabbix1:~# wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian11_all.deb
root@zabbix1:~# dpkg -i zabbix-release_latest_6.0+debian11_all.deb
root@zabbix1:~# apt update
```
3) Установаю Zabbix сервер, веб-интерфейс и, агент на ВМ Zabbix1
```
root@zabbix1:~# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```
4) Создаю пользователя БД zabbix
```
root@zabbix1:~# sudo -u postgres createuser --pwprompt zabbix
```
5) Создаю БД zabbix для пользователя zabbix
```
root@zabbix1:~# sudo -u postgres createdb -O zabbix zabbix
```
6) импортирую схему и данные
```
root@zabbix1:~# zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
7) в конфигурационном файле устанавливаем пароль к БД
```
root@zabbix1:~# sudo nano /etc/zabbix/zabbix_server.conf
```
8) Перезапускаем процессы Zabbix сервера и агента
```
root@zabbix1:~# root@zabbix1:~# systemctl restart zabbix-server zabbix-agent apache2
root@zabbix1:~# systemctl enable zabbix-server zabbix-agent apache2
```
![zabbix1](https://github.com/DefAKAAlex/zabbix1/blob/main/img/install-end.png)


Получаем установленный и настроенный Zabbix сервер на ВМ Zabbix1, на этой же ВМ установлен агент.

Заходим в веб-интерфейс сервера Zabbix

![zabbix1](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix1.png)

![zabbix2](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix2.png)

![zabbix3](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix3.png)

![zabbix4](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix4.png)


## Задание 2

`Установите Zabbix Agent на два хоста.`

```
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
```

В первой части задания, одновременно с сервером, на ВМ Zabbix1 был установлен zabbix агент.
На ВМ Zabbix2 устанавливаем только агента, так же с подсказкой сайта https://www.zabbix.com/

![PrefAgent](https://github.com/DefAKAAlex/zabbix1/blob/main/img/pref_agnt.png)

1) Устанавливаем репозиторий Zabbix
```
root@zabbix2:~# wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian11_all.deb
root@zabbix2:~# dpkg -i zabbix-release_latest_6.0+debian11_all.deb
root@zabbix2:~# apt update
```
2) Установаю Zabbix агент на ВМ Zabbix2
```
root@zabbix2:~# root@zabbix1:~# apt install zabbix-agent
```

Агенты установлены.
Подключаем их к серверу.

![zabbix1](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix2-1.png)

![zabbix2](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix2-2.png)

![zabbix3](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix2-3.png)

![zabbix4](https://github.com/DefAKAAlex/zabbix1/blob/main/img/zabbix2-4.png)
