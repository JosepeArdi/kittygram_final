# Kittygram

[![Main Kittygram workflow](https://github.com/Josepeardi/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/Josepeardi/kittygram_final/actions/workflows/main.yml)

[![Website](https://img.shields.io/website?url=https%3A%2F%2Fmaufen.zapto.org%2F)](https://maufen.zapto.org/)


![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) ![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

## Описание
- Проект Kittygram позволяет пользователям делиться своими фотографиями котиков и просматривать фотографии котиков других пользователей.
- При загрузке фото котика, пользователь должен ввести имя котика, год его рождения. При желании, может добавить ему достижения.
- Добавлять и просматривать фотографии могут только зарегистрированные и авторизованные пользователи.
- Только авторы могут изменять фотографии своих питомцев и описание.

## Технологии
- Python
- Docker
- PostgreSQL
- Django REST framework
- Gunicorn
- Nginx
- React

## Локальное развертывание

Клонировать репозиторий:
```shell
git clone git clone <https or SSH URL>
```
Перейти в каталог проекта:
```shell
cd kittygram
```

Создать .env файл со следующим содержанием:
```shell
# DB
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=5432

# Django settings
SECRET_KEY=<django secret key>
DEBUG=False
ALLOWED_HOSTS=127.0.0.1;localhost;<example.com;xxx.xxx.xxx.xxx>

```
Развернуть приложение:
```shell
sudo docker compose -f docker-compose.yml up
```
После успешного запуска, проект доступен на локальном IP `127.0.0.1:9000`.

## Развертывание на удаленном сервере
Для развертывания на удаленном сервере необходимо клонировать репозиторий на локальную машину. Подготовить и загрузить образы на Docker Hub.

Клонировать репозиторий:
```shell
git clone git clone <https or SSH URL>
```
Перейти в каталог проекта:
```shell
cd kittygram
```

Создать docker images образы:
```shell
sudo docker build -t <username>/kittygram_backend backend/
sudo docker build -t <username>/kittygram_frontend frontend/
sudo docker build -t <username>/kittygram_gateway nginx/
```
Загрузить образы на Docker Hub:
```shell
sudo docker push <username>/kittygram_backend
sudo docker push <username>/kittygram_frontend
sudo docker push <username>/kittygram_gateway
```
Создать .env файл со следующим содержанием:
```shell
# DB
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=5432

# Django settings
SECRET_KEY=<django secret key>
DEBUG=False
ALLOWED_HOSTS=127.0.0.1;localhost;<example.com;xxx.xxx.xxx.xxx>

```

💡 Дальнейшая инструкция предполагает, что удаленный сервер настроен на работу по `SSH`.
На сервере установлен Docker. Установлен и настроен nginx в качестве балансировщика
нагрузки.

Перенести на удаленный сервер файлы `.env` `docker-compose.production.yml`:
```shell
scp .env docker-compose.production.yml <user@server-address>:/home/<user name>/kittygram
```
Подключиться к серверу:
```shell
ssh user@server-address
```
Перейти в директорию `kittygram`:
```shell
cd /home/<user name>/kittygram
```
Выполнить сборку приложений:
```shell
sudo docker compose -f docker-compose.production.yml up -d
```
Выполнить миграции:
```shell
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```
Собрать статику:
```shell
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
```
Скопировать статику:
```shell
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. web/backend_static/static
```
Настроить шлюз для перенаправления запросов на `9000` порт, который слушает контейнер `kittygram_gateway`
```shell
sudo  nano /etc/nginx/sites-enabled/default
```
Пример конфигурации Nginx:
```shell
server {
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:9000;
    }
}


```
Получить и настроить SSL-сертификат вашим любимым способом.

Перезапустить nginx:
```shell
sudo systemctl restart nginx.service
```

## GitHub Actions
Для использования автоматизированного развертывания и тестирования нужно 
изменить `.github/workflows/main.yml` под свои параметры и аккаунт.

Actions secrets:
- `secrets.DOCKER_USERNAME`
- `secrets.DOCKER_PASSWORD`
- `secrets.HOST`
- `secrets.USER`
- `secrets.SSH_KEY`
- `secrets.SSH_PASSPHRASE`
- `secrets.TELEGRAM_TO`
- `secrets.TELEGRAM_TOKEN`


### Автор проекта

[Андрей Бузинов](https://github.com/JosepeArdi)