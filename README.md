> Источники: 
> [w0rng/amnezia-wg-easy: The easiest way to run WireGuard VPN + Web-based Admin UI.](https://github.com/w0rng/amnezia-wg-easy?tab=readme-ov-file) - здесь же про пароль, `docker-compose`
> 
> [wg-easy/wg-easy: The easiest way to run WireGuard VPN + Web-based Admin UI.](https://github.com/wg-easy/wg-easy) - это уже про обычный `Wireguard`

---
# **0. Создание хеша пароля для авторизации.**

> Решение проблемы с паролем - [PASSWORD · Issue #1127 · wg-easy/wg-easy](https://github.com/wg-easy/wg-easy/issues/1127?ysclid=m5fy0gl4o1653663191)

Хеш пароля генерируется командой:
```bash
docker run ghcr.io/wg-easy/wg-easy wgpw kk
```
где `kk` - пароль от WEB UI.
>Если вдруг чего не але - попробовать пароль вписывать в одинарные кавычки

После нужно добавить этот хеш в `.env`, в параметр `PASSWORD_HASH`, объединив **<u>одинарными кавычками</u>**, никаких вторых `$`!.

Общий вид `.env`:

```env
LANG=en
WG_HOST=10.6.1.251
PASSWORD_HASH='$2a$12$22z505rWWMua1roUCachqODQcQaqOUrHP/KSgZFY.zmb9r//3dqIu'
PORT=34345
WG_PORT=15559
```

---
# **1. Настройка сервера и WEB UI для Wireguard.**
---
Предварительно закинуть на сервак папочку `amnezia`, лежит у меня в `D:\temp_for_machines`

Также стоит переопределить порты в `.env`:
- `PORT` - порт для WEB UI;
- `WG_PORT` - порт для самого WG.
```bash
cd amnezia
docker-compose up -d
```
После заходим, выдаем конфиги, радуемся жизни.
> Пинги проходят между всеми клиентами, что не может не радовать



## Дописать как изменить сеть, и прочие приколы которые 100% понадобятся.
