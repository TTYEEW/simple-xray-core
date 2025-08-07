# Скрипт для лёгкой установки и настройки ядра X-ray без графического интерфейса

Вы все знакомы с такими панелями управления, как 3x-ui, Marzban и другими. Все эти панели являются всего лишь графическими надстройками над ядром X-ray и служат для удобного управления им, а также для создания подключений и настроек. Ядро же может работать без всяких панелей и управляться полностью через терминал. Основное преимущество использования «голого» ядра заключается в том, что вам не нужно заморачиваться с доменами и TLS-сертификатами. Само ядро можно установить и администрировать вручную с помощью официальной документации. Этот скрипт предназначен для упрощения этой задачи: он автоматически установит ядро на сервер, создаст конфигурационные файлы и несколько исполняемых файлов для удобного управления пользователями.

## Системные требования

- 1 CPU  
- 1 GB RAM  
- 10 GB диска  
- ОС Ubuntu 22 x64

## Как пользоваться скриптом

Для запуска скрипта используйте следующую команду:

```bash
wget https://raw.githubusercontent.com/TTYEEW/simple-xray-core/main/xray-install -O xray-install
chmod +x xray-install
./xray-install
```
> ⚠️ Скрипт протестирован на Ubuntu 22 и 24. На других дистрибутивах работа не гарантируется.

## Конфигурация Xray и маршрутизация через WARP

После установки создаётся основной конфигурационный файл Xray:

```
/usr/local/etc/xray/config.json
```

В него уже включена настройка маршрутов с использованием Cloudflare WARP. По умолчанию, через WARP направляется трафик:

- к государственным доменам
- к доменам с окончаниями `.ru` и `.su`
- к `openai.com`

Пример правила маршрутизации:

```json
"rules": [
  {
    "type": "field",
    "domain": [
      "geosite:category-gov-ru",
      "regexp:.*\\.ru$",
      "regexp:.*\\.su$",
      "geosite:openai"
    ],
    "outboundTag": "warp"
  }
]
```

## Команды для управления пользователями

**Вывести список всех клиентов:**

```bash
userlist
```

**Вывести ссылку и QR-код для подключения основного пользователя:**

```bash
mainuser
```

**Создать нового пользователя:**

```bash
newuser
```

**Удалить пользователя:**

```bash
rmuser
```

**Создать ссылку для подключения:**

```bash
sharelink
```

Подсказки по командам также доступны в домашней директории в файле `help`:

```bash
cat help
```

## Удаление Xray и WARP

Если потребуется удалить всё:

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ remove
rm /usr/local/etc/xray/config.json
rm /usr/local/etc/xray/.keys
rm /usr/local/bin/userlist
rm /usr/local/bin/mainuser
rm /usr/local/bin/newuser
rm /usr/local/bin/rmuser
rm /usr/local/bin/sharelink
warp-cli disconnect
warp-cli unregister
systemctl stop warp-svc
systemctl disable warp-svc
apt purge --autoremove cloudflare-warp -y
```

## Полезные ссылки

- [GitHub проекта X-ray Core](https://github.com/XTLS/Xray-core)
- [Официальная документация на русском](https://xtls.github.io/ru/)

## Клиенты для подключения

**Windows**

- [v2rayN](https://github.com/2dust/v2rayN)  
- [Furious](https://github.com/LorenEteval/Furious)  
- [Invisible Man - Xray](https://github.com/InvisibleManVPN/InvisibleMan-XRayClient)  

**Android**

- [v2rayNG](https://github.com/2dust/v2rayNG)  
- [X-flutter](https://github.com/XTLS/X-flutter)  
- [SaeedDev94/Xray](https://github.com/SaeedDev94/Xray)  

**iOS & macOS arm64**

- [Streisand](https://apps.apple.com/app/streisand/id6450534064)  
- [Happ](https://apps.apple.com/app/happ-proxy-utility/id6504287215)  
- [OneXray](https://github.com/OneXray/OneXray)  

**macOS arm64 & x64**

- [V2rayU](https://github.com/yanue/V2rayU)  
- [V2RayXS](https://github.com/tzmax/V2RayXS)  
- [Furious](https://github.com/LorenEteval/Furious)  
- [OneXray](https://github.com/OneXray/OneXray)  

**Linux**

- [Nekoray](https://github.com/MatsuriDayo/nekoray)  
- [v2rayA](https://github.com/v2rayA/v2rayA)  
- [Furious](https://github.com/LorenEteval/Furious)  