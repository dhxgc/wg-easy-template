# Пример простого развертывания в дефолтной конфигурации.

> Если не менять хеш - могут возникнуть проблемы.
>
> Если что-то не получилось:
```
docker kill amnezia-wg-easy
docker rm amnezia-wg-easy
docker volume rm etc_wireguard
```
> После поднимаем заново.


1. Качаем необходимое:
```bash
apt install git docker.io docker-compose
git clone https://github.com/dhxgc/wg-easy-template.git && cd wg-easy-template/amnezia-default
```

2. Генерируем хеш:
```bash
root@debian:~# docker run --rm -it ghcr.io/wg-easy/wg-easy wgpw P@ssw0rd
PASSWORD_HASH='$2a$12$TZqVW0tSmuhcuD1th.yg0Om/zXLRYf5nOu3WI3nEWH/d26X2DNlGi'
```

3. Приводим файл `.env` к виду, вставляем хеш из п.2, указываем свой IP:
```
LANG=en
WG_HOST=10.6.1.251
  # Template for password = kk
PASSWORD_HASH='$2a$12$TZqVW0tSmuhcuD1th.yg0Om/zXLRYf5nOu3WI3nEWH/d26X2DNlGi'
PORT=14881
WG_PORT=14880
```

4. В файле `docker-compose.yml` меняем порты:
```
volumes:
  etc_wireguard:

services:
  amnezia-wg-easy:
    env_file:
      - .env
    image: ghcr.io/w0rng/amnezia-wg-easy
    container_name: amnezia-wg-easy
    volumes:
      - etc_wireguard:/etc/wireguard
    ports:
      - "14880:14880/udp"
      - "14881:14881/tcp"
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
    devices:
    - /dev/net/tun:/dev/net/tun
```
