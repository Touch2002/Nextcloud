# Працюючий nextcloud(Тільки в локальній мережі)

Тут буде описано як я встановив хмарне сховище nextcloud на віртуальну машину з операційною системою лінукс
## 0. всі команди я виконував з правами рут для тих хто не знає запускаєтся командою:
```
sudo su
``` 

## 1. Щоб завантажити пакет Nextcloud snap і встановити його, введіть:
___
```
sudo snap install nextcloud
```
Пакет Nextcloud буде завантажений і всановлений на ваш сервер. Ви можете перевірити успішнісь встановлення за допомогою виводу змін, поа'язаних з пакетом snap:
```
snap changes nextcloud
```
Це повідомлення повинно з'явитися посля команди вище.
```
Output
ID   Status  Spawn               Ready               Summary
2    Done    today at 16:12 UTC  today at 16:12 UTC  Install "nextcloud" snap
```
## 2. Встановлення і налаштування облікового запису адміністратора
___
```
nextcloud.manual-install admin password
```
Замість admin пишемо свій логін, а замість password свій пароль 

Наступне повідомлення показує, що налаштування Nextcloud виконано правильно:

```
Output
Nextcloud is not installed - only a limited number of commands are available
Nextcloud was successfully installed
```
## 3. Налаштування довірених доменів
___
Можливо продивитись поточні налаштування, запросивши значення масиву trusted_domains:
```
nextcloud.occ config:system:get trusted_domains
```
Це повідомлення з'явится посля команди вище 
```
Output
localhost
```
В поточний час в масиві зберігається тільки перше значення localhost. Ми можемо додати запис для доменного им'я або IP-адреси нашого сервера, ввівши наступне:
```
nextcloud.occ config:system:set trusted_domains 1 --value=example.com
```
Замість example.com пишемо айпі адресу віртуальної машини.

Посля цієї команди ми повинні отримати повідомлення 
```
Output
System config value trusted_domains => 1 set to string example.com
```
Якщо ми знову введемо команду nextcloud.occ config:system:get trusted_domains ми побачимо вже два записи:
```
Output
localhost
example.com
```
Замість example.com буде наш айпі.

## 4. Налаштування SSL, с сертифікатом з особистим підписом.
___

Щоб сгенерувати сертифікат з особистим підписом і налаштувати Nextcloud для його використання, введіть:
```
nextcloud.enable-https self-signed
```
Показаний нижче результат означає, що Nextcloud сгенерував і активував сертифікат з особистим підписом.
```
Output
Generating key and self-signed certificate... done
Restarting apache... done
```
Тепер, коли ми захистили інтерфейс, відкрийте веб-порти брандмауера, щоб дозволити доступ до веб-інтерфейсу:
```
ufw allow 80,443/tcp
```
## 5. Вхід в веб-інтерфейс Nextcloud
___ 
Вхід в nextcloud доступний по айпі адресі виртуальної машини. Логін і пароль ми вводили першою командою в другому кроці.

___
На цьому пока все, сервер працює в локальній мережі.
## Джерела
___
- Туторіал [тут.](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-nextcloud-on-ubuntu-18-04-ru)

- [Відео](https://youtu.be/XYataiVYUKo) де автор виконував все чітко з мануалом. 
- [Відео від автору канала "черный треуголинк" по якому не вийшло створити сервер nextcloud](https://youtu.be/aJ3gHHFATyc)