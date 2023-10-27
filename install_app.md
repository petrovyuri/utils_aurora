<!-- # Установка приложения на Аврору

#### Обновление зависимостей
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
 -->
