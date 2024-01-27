# Запуск Postgresql в docker контейнере.

> Убедитесь, что в системе установлен docker: ```docker --version``` и psql ```psql --version```.

## Способ 1. docker-compose внутри проекта

Содержимое файла `docker-compose.yml`:

```yaml
version: '3.1'

services:

  db:
    image: postgres:latest
    restart: always
    ports:
      - "5433:5432"
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
```

Рядом создаем файл `.env` с вашими данными:

```env
POSTGRES_DB=demo-db
POSTGRES_USER=admin
POSTGRES_PASSWORD=hackme
```

Запускаем контейнер:

```bash
docker compose up
```

И подключаемся при помощи psql:

```bash
psql -h [HOSTNAME] -p [PORT] -U [USERNAME] -d [DATABASENAME]`
```