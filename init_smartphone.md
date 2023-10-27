# Подключение к телефону

### Активация раежима разработчика
Данная операция активирует режим разработчика, позволяющий выполнять код в режиме разработки. При активации режима разработчика, можно нанести телефону не поправимый ущерб. Так, что будьте осторожны. 
Далее, нам необходимо настроить SSH для подключения
Выполняем по этой инструкции: 1 и 2 пункт.
https://developer.auroraos.ru/doc/software_development/sdk/device

#### Установка пакетов совместимости
Данные пакеты, необходимо установить, что бы текущая версия Авроры, могла работать с Flutter приложением. В дальнейшем, в SDK будут добавлены эти пакеты и их не надо будет вручную устанавливать.
Данные пакеты лежат в папке <Папка с Flutter>/bin/cache/artifacts/aurora/arm/device/compatibility
Там два файла:
**maliit-framework-wayland-0.99.1+git12-1.7.25.omp.armv7hl.rpm**
**maliit-framework-wayland-inputcontext-0.99.1+git12-1.7.25.omp.armv7hl.rpm**

#### Копирование файлов в папку Downloads в смартфоне
```shell
scp $HOME/.local/opt/flutter/bin/cache/artifacts/aurora/arm/device/compatibility/maliit-framework-wayland-0.99.1%252Bgit12-1.7.25.omp.armv7hl.rpm defaultuser@192.168.2.15:/home/defaultuser/Downloads

scp $HOME/.local/opt/flutter/bin/cache/artifacts/aurora/arm/device/compatibility/maliit-framework-wayland-inputcontext-0.99.1%252Bgit12-1.7.25.omp.armv7hl.rpm defaultuser@192.168.2.15:/home/defaultuser/Downloads
```

#### Подключение к телефону через ssh
```shell
defaultuser@192.168.2.15
```
Вводим пароль который вы установили в режиме разработчика

#### Переходим в рут
```shell
pkcon install-local /home/defaultuser/Downloads/maliit-framework-wayland-inputcontext-0.99.1%252Bgit12-1.7.25.omp.armv7hl.rpm -y

pkcon install-local /home/defaultuser/Downloads/maliit-framework-wayland-0.99.1%252Bgit12-1.7.25.omp.armv7hl.rpm -y
```
Выходим.
```shell
exit
```

Всё, пакеты установлены, можно приступать к разработке приложений