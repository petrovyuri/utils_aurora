#### Клонируем проект flutter_plugins
```shell
https://gitlab.com/omprussia/flutter/flutter-plugins
```

#### Установка внешнего отладчика
## Почитать про отладчик
https://sourceware.org/gdb/papers/multi-arch/whatis.html

```shell
sudo apt install gdb-multiarch
```

#### Создаем файл конфигурации 
.gdbinit

Добавляем туду команду: 
```shell
set remote exec-file /usr/bin/ru.auroraos.flutter_example_packages
```

#### Установка плагина Native-Debug
https://marketplace.visualstudio.com/items?itemName=webfreak.debug

#### Создаем файл конфигурации для запуска внешнего отладчика
```shell
{
    "version": "0.2.0",
    "configurations": [
            {
                "name": "Attach with GDB",
                "type": "cppdbg",
                "request": "launch",
                "program": "./УКАЗАТЬ ИСПОЛНЯЕМЫЙ ФАЙЛ",
                "MIMode": "gdb",
                "miDebuggerPath": "/usr/bin/gdb-multiarch",
                "miDebuggerServerAddress": "192.168.2.15:10000",
                "useExtendedRemote": true,
                "cwd": "ПАПКА ПРОЕКТА",
            }
        ]
}
```
#### Сборка приложения в режиме DEBUG
```shell
flutter-aurora build aurora --debug
```
#### Копирование файлов
```shell
scp $HOME/ПУТЬ К файлам defaultuser@192.168.2.15:/home/defaultuser/Downloads
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
gdbserver --multi :10000    
```

