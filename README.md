# What is Docker-YaTank

Docker-YaTank is a [Yandex.Tank](https://tech.yandex.ru/tank/) container for load testing.
This container is based on [ErgoZ's one](https://github.com/RiftBit/docker-yatank), but uses PyPI Tank package instead of .deb

# How to use

## Usable params
- **-h, --help** - show command line options 
- **-c CONFIG, --config=CONFIG** - read options from INI file. It is possible to set multiple INI files by specifying the option serveral times. Default: ``/yandex-example-configs/load.iniload.ini`` 
- **-i, --ignore-lock** - ignore lock files 
- **-f, --fail-lock** - don't wait for lock file, quit if it's busy. The default behaviour is to wait for lock file to become free. 
- **-l LOG, --log=LOG** - main log file location. Default: ``./tank.log``
- **-n, --no-rc** - don't read ``/etc/yandex-tank/*.ini`` and ``~/.yandex-tank``
- **-o OPTION, --option=OPTION** - set an option from command line. Options set in cmd line override those have been set in configuration files. Multiple times for multiple options. Format: ``<section>.<option>=value`` Example: ``yandex-tank -o "console.short_only=1" --option="phantom.force_stepping=1"``
- **-s SCHEDULED_START, --scheduled-start=SCHEDULED_START** - run test on specified time, date format YYYY-MM-DD hh:mm:ss or hh:mm:ss
- **-q, --quiet** - only print WARNINGs and ERRORs to console 
- **-v, --verbose** - print ALL messages to console. Chatty mode

Add an ammo file name as a nameless parameter, e.g.:
``yandex-tank ammo.txt`` or ``yandex-tank ammo.gz``

## Example load.ini
ATENTION!!! Main folder is ``/yandex-tank-configs`` in container. All paths in load.ini must follow this folder! See ammo example config below.

```
[phantom]
address=example.com:80 ; Target's address and port
rps_schedule=const(5,1m) ; load scheme
header_http = 1.1
headers = [Host: example.com] [Cookies: None] [Connection: close]
ammo = /yandex-tank-configs/ammo.txt
```

## Pull and Run the Docker Container


```
docker run --name yatank -ti --rm -v /path-to-config-dir:/yandex-tank-configs direvius/docker-yatank -c /yandex-tank-configs/load.ini
```

or

```
docker run --name yatank -ti --rm -v /path/to/config/dir:/yandex-tank-configs direvius/docker-yatank /yandex-tank-configs/ammo.txt
```


**All report files will be created in /path/to/config/dir used for mount config.**

