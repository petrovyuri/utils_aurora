# Установка Platform SDK + Flutter Aurora на Linux (Ubuntu)

[Flutter aurora](https://gitlab.com/omprussia/flutter/flutter)
 [Platform SDK](https://developer.auroraos.ru/doc/software_development/psdk/setup)

## Создание нового пользователя dev
```shell
useradd -m dev
```
## Добавление пользователя dev в группу Sudo
```shell
usermod -aG sudo myuser
```
## Переключение на пользователь dev
```shell
su dev
```


## Установка Flutter Aurora

#### Обновление зависимостей
```shell
sudo apt-get update
```
#### Получение зависимостей
```shell
sudo apt-get install curl git git-lfs unzip bzip2
```
#### Создание локальной папки для хранения Flutter
```shell
mkdir -p ~/.local/opt
```
#### Копирование Flutter на компьютер
```shell
git clone https://gitlab.com/omprussia/flutter/flutter.git ~/.local/opt/flutter

echo "alias flutter-aurora=$HOME/.local/opt/flutter/bin/flutter" >> ~/.bashrc

exec bash  
``` 
#### Активация Flutter Авроры
```shell
flutter-aurora config --enable-aurora
``` 
#### Проверка установки
```shell
flutter-aurora doctor
```

## Установка Platform SDK

#### Инициализация переменных для скачивание
```shell
URL_CHROOT="https://sdk-repo.omprussia.ru/sdk/installers/4.0.2/PlatformSDK/4.0.2.249/Aurora_OS-4.0.2.249-base-Aurora_Platform_SDK_Chroot-i486.tar.bz2"
URL_TOOLING="https://sdk-repo.omprussia.ru/sdk/installers/4.0.2/PlatformSDK/4.0.2.249/Aurora_OS-4.0.2.249-base-Aurora_SDK_Tooling-i486.tar.bz2"
URL_TARGET_ARM="https://sdk-repo.omprussia.ru/sdk/installers/4.0.2/PlatformSDK/4.0.2.249/Aurora_OS-4.0.2.249-base-Aurora_SDK_Target-armv7hl.tar.bz2"
NAME=$(basename $URL_TOOLING | sed s/.tar.[a-z]*[0-9]*//g | sed s/-base-Aurora_SDK_Tooling-i486//g )
```
#### Скачивание зависимостей
```shell
if [ ! -f $(basename $URL_CHROOT) ]; then
  wget $URL_CHROOT
fi

if [ ! -f $(basename $URL_TOOLING) ]; then
  wget $URL_TOOLING
fi

if [ ! -f $(basename $URL_TARGET_ARM) ]; then
  wget $URL_TARGET_ARM
fi
```
#### Создание папок
```shell
mkdir -pv $HOME/AuroraPlatformSDK/tarballs
mkdir -pv $HOME/AuroraPlatformSDK/sdks/aurora_psdk
mkdir -pv $HOME/AuroraPlatformSDK/projects
```
#### Переходим в папку для установки
```shell
cp -v $(pwd)/Aurora_OS-*.bz2 $HOME/AuroraPlatformSDK/tarballs
```
#### Добавление путей в PATH
```shell
if [[ -z $(grep "AuroraPlatformSDK" ~/.bashrc) ]]; then
  echo 'export PSDK_DIR=$HOME/AuroraPlatformSDK/sdks/aurora_psdk' >> $HOME/.bashrc
fi

if [[ -z $(grep "alias aurora_psdk" ~/.bashrc) ]]; then
  echo 'alias aurora_psdk=$PSDK_DIR/sdk-chroot' >> ~/.bashrc
fi

echo 'PS1="[AuroraPlatformSDK]$ "' > ~/.mersdk.profile
```
#### Установка Platform SDK
```shell
export PSDK_DIR=$HOME/AuroraPlatformSDK/sdks/aurora_psdk
export CHROOT_IMG=$(find $HOME/AuroraPlatformSDK/tarballs -iname "*chroot*")

sudo tar --numeric-owner -p -xjf $CHROOT_IMG --checkpoint=.1000 -C $PSDK_DIR
$PSDK_DIR/sdk-chroot sdk-assistant tooling create \
  $NAME \
  $HOME/AuroraPlatformSDK/tarballs/$NAME-base-Aurora_SDK_Tooling-i486.tar.bz2

$PSDK_DIR/sdk-chroot sdk-assistant target create \
  $NAME-armv7hl \
  $HOME/AuroraPlatformSDK/tarballs/$NAME-base-Aurora_SDK_Target-armv7hl.tar.bz2

$PSDK_DIR/sdk-chroot sdk-assistant list
```
#### Установка таргетов
```shell
TARGET="$NAME-armv7hl"

if [ ! -d "platform-sdk" ]; then
  git clone git@gitlab.com:omprussia/flutter/flutter.git
  mv ./flutter/bin/cache/artifacts/aurora/arm/platform-sdk ./platform-sdk
  rm -rf ./flutter
fi

$PSDK_DIR/sdk-chroot \
  sb2-config -d \
  $TARGET

$PSDK_DIR/sdk-chroot \
  sb2 -t $TARGET -m sdk-install -R zypper in platform-sdk/compatibility/*.rpm

$PSDK_DIR/sdk-chroot \
  sb2 -t $TARGET -m sdk-install -R zypper in platform-sdk/*.rpm

$PSDK_DIR/sdk-chroot \
  sdk-assistant target remove --snapshots-of $TARGET
```
```shell
bash
```