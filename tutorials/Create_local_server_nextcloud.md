# Работающий nextcloud(только в локальной сети)

Здесь будет описано как я установил облачное хранилище nextcloud на виртуальной машине
## 0. все команды я выполнял из под судо для тех кто не знает запускается командой:
```
sudo su
``` 

## 1. Чтобы загрузить пакет Nextcloud snap и установить его в системе, введите:
___
```
sudo snap install nextcloud
```
Пакет Nextcloud будет загружен и установлен на ваш сервер. Вы можете проверить успешность установки посредством вывода изменений, связанных с пакетом snap:
```
snap changes nextcloud
```
Это какое сообщение должно появится после команды выше.
```
Output
ID   Status  Spawn               Ready               Summary
2    Done    today at 16:12 UTC  today at 16:12 UTC  Install "nextcloud" snap
```
## 2. Установка и настройка учетной записи администратора
___
```
nextcloud.manual-install admin password
```
Dместо admin пишем свой логин, а после password свой пароль 

Следующее сообщение указывает, что настройка Nextcloud выполнена правильно:

```
Output
Nextcloud is not installed - only a limited number of commands are available
Nextcloud was successfully installed
```
## 3. Настройка доверенных доменов
___
Можно просмотреть текущие настройки, запросив значение массива trusted_domains:
```
nextcloud.occ config:system:get trusted_domains
```
Это сообщение появитсься после команды выше 
```
Output
localhost
```
В настоящее время в массиве содержится только первое значение localhost. Мы можем добавить запись для доменного имени или IP-адреса нашего сервера, введя следующее:
```
nextcloud.occ config:system:set trusted_domains 1 --value=example.com
```
Вместо example.com пишем айпи адрес виртуальной машины.

После этой команды мы должны получить сообщение 
```
Output
System config value trusted_domains => 1 set to string example.com
```
Если мы снова введем команду nextcloud.occ config:system:get trusted_domains мы увидим уже две записи:
```
Output
localhost
example.com
```
Вместо example.com будет наш айпи.

## 4. Настройка SSL с сертификатом с собственной подписью.
___

Чтобы сгенерировать сертификат с собственной подписью и настроить Nextcloud для его использования, введите:
```
nextcloud.enable-https self-signed
```
Показанный ниже результат означает, что Nextcloud сгенерировал и активировал сертификат с собственной подписью.
```
Output
Generating key and self-signed certificate... done
Restarting apache... done
```
Теперь, когда мы защитили интерфейс, откройте веб-порты брандмауэра, чтобы разрешить доступ к веб-интерфейсу:
```
ufw allow 80,443/tcp
```
## 5. Вход в веб-интерфейс Nextcloud
___ 
Вход в nextcloud доступен по айпи адресу виртуальной машины. Логин и пароль мы вводили первой командой во втором шаге.

___
На этом пока все, сервер работает ~~**Почти**~~.
## Источники 
___
- Туториал [Здесь.](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-nextcloud-on-ubuntu-18-04-ru)

- [Видео](https://youtu.be/XYataiVYUKo) где автор выполнял все в четкости с мануалом. 
- [Видео от автора канала черный треуголинк по которому не получилось создать сервер nextcloud](https://youtu.be/aJ3gHHFATyc)