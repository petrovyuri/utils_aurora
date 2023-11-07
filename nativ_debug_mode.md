### Часть 1, установка debug пакетов

#### Клонируем проект flutter_plugins
```shell
https://gitlab.com/omprussia/flutter/flutter-plugins
```
#### Переходим в папку example и запускаем кодогенерацию
```shell
cd example
flutter-aurora pub get
dart run build_runner build --delete-conflicting-outputs
```
#### Сборка приложения в режиме DEBUG
```shell
flutter-aurora build aurora --debug
```
После сборки вам необходимо подписать все три пакета
```shell
aurora_psdk rpmsign-external sign --key $HOME/sign/regular_key.pem --cert  $HOME/sign/regular_cert.pem $HOME/flutter-plugins/example/build/aurora/aurora-arm/debug/RPMS/*.rpm
```
Копируем все пакеты в смартфон
```shell
scp $HOME/flutter-plugins/example/build/aurora/aurora-arm/debug/RPMS/*.rpm defaultuser@192.168.2.15:/home/defaultuser/Downloads
```
#### Подключение к смартфону
```shell
ssh defaultuser@192.168.2.15
```
#### Переход в режим супер пользователя
```shell
devel-su
```
#### Установка всех пакетов из папки Downloads
```shell
pkcon install-local /home/defaultuser/Downloads/*.rpm -y
```
#### Выход из супер пользователя
```shell
exit    
```
#### Запускаем GDB Server
```shell
gdbserver --multi :10001    
```

### Часть 2, настройка внешнего отладчика

#### Установка внешнего отладчика
## Почитать про отладчик
https://sourceware.org/gdb/papers/multi-arch/whatis.html

```shell
sudo apt install gdb-multiarch
```
#### Установка плагина С++
https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-extension-pack

#### Установка плагина Native-Debug
https://marketplace.visualstudio.com/items?itemName=webfreak.debug

#### Создаем файл конфигурации 
.gdbinit

Добавляем туда команду: 
```shell
set remote exec-file /usr/bin/ru.auroraos.flutter_example_packages
```

#### Создаем файл конфигурации для запуска внешнего отладчика
```shell
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach with GDB",
            "type": "cppdbg",
            "request": "launch",
            "program": "./example/build/aurora/aurora-arm/debug/ru.auroraos.flutter_example_packages",
            "MIMode": "gdb",
            "miDebuggerPath": "/usr/bin/gdb-multiarch",
            "miDebuggerServerAddress": "192.168.2.15:10001",
            "useExtendedRemote": true,
            "cwd": "${workspaceRoot}",
         }
    ]
}
```



