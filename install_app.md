# Установка приложения на Аврору

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
aurora_psdk rpmsign-external sign --key $HOME/sign/regular_key.pem --cert $HOME/sign/regular_cert.pem $PATH_TO_APP
```

#### Копирование приложения в смартфон
```shell
scp $PATH_TO_APP  defaultuser@192.168.2.15:/home/defaultuser/Downloads
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