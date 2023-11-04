# Установка приложения на Аврору

#### Установка редактора nano
```shell
sudo apt-get install nano
```

#### Так как сборка выполняется в консоли, не всегда удобно каждый раз вводить пароль вручную. Для решения этой проблемы нужно добавить следующие файлы в директорию /etc/sudoers.d, они позволят работать с Platform SDK без ввода пароля суперпользователя.

```shell
sudo nano mer-sdk-chroot
```

Добавляем туда:

dev ALL=(ALL) NOPASSWD: /home/dev/AuroraPlatformSDK/sdks/aurora_psdk/mer-sdk-chroot  
Defaults!/home/dev/AuroraPlatformSDK/sdks/aurora_psdk/mer-sdk-chroot env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"  

#### Создаем еще один
```shell
sudo nano sdk-chroot
```
Добавляем туда:

dev ALL=(ALL) NOPASSWD: /home/dev/AuroraPlatformSDK/sdks/aurora_psdk/sdk-chroot  
Defaults!/home/dev/AuroraPlatformSDK/sdks/aurora_psdk/sdk-chroot env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"  

После этого, вам не надо будет вводить пароль суперпользователя для сборки

#### Создание приложения для Авроры
```shell
flutter-aurora create --platforms=aurora --template=app --org=com.example example
```

#### Добавление поддержки Авроры
```shell
flutter-aurora create --platforms=aurora --org=com.example .
```

#### Сборка приложения
```shell
flutter-aurora build aurora --release
```

#### Для удобства, копируете полную ссылку на файл RPM
Данная файл, это и есть ваше приложение. Расширение файла .rpm.
Его местоположение после сборки аходится:
<папка проекта>/build/aurora/arm/release/RPMS/
пример:
com.example.mobile-0.1.0-1.armv7hl.rpm

#### Инициализируем переменную  
```shell
PATH_TO_APP= 
```

#### Подпись приложения
Скачать тестовый сертификат и ключ можно на сайте.
https://developer.auroraos.ru/doc/software_development/guides/package_signing#public_certificates

После того, как скачали сертификат и ключ, ложите их в папку sign в корневой каталог пользователя. Далее подписываем приложение командой:
```shell
aurora_psdk rpmsign-external sign --key <ПУТЬ к КЛЮЧУ> --cert <ПУТЬ к СЕРТИФИКАТУ> $PATH_TO_APP
```

#### Копирование приложения в смартфон
```shell
scp $PATH_TO_APP  defaultuser@192.168.2.15:/home/defaultuser/Downloads
```

#### Подключение к смартфону
```shell
ssh defaultuser@192.168.2.15
```

#### Переход в режим ROOT 
Так, как в этом режиме вы получаете полный доступ. Будьте аккуратны, что бы не поломать смартфон.
```shell
devel-su
```
Вводим пароль который вы установили в при активации терминала

#### Установка пакета
```shell
pkcon install-local /home/defaultuser/Downloads/*.rpm -y
```