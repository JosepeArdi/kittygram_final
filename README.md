Kittygram в контейнерах и CI/CD с помощью GitHub Actions
Описание
Социальная сеть для обмена фотографиями любимых питомцев. Проект состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

Технологии
Django 3.2.3 | djangorestframework 3.12.4 | djoser 2.1.0 | webcolors 1.11.1 | Pillow 9.0.0 | pytest 6.2.4 | pytest-django 4.4.0 | pytest-pythonpath 0.7.3 | python-dotenv 0.19.0

Деплой проекта на удалённый сервер:
Клонировать репозиторий и перейти в него в командной строке:

git clone https://github.com/josepeardy/kittygram_final.git
cd kittygram_final/
Создать файл .env и заполнить его по образцу .env.template.

Перейдите в директорию бэкенд-приложение проекта:

cd backend/
Создать и активировать виртуальное окружение:

python3 -m venv venv
source env/bin/activate
python -m pip install --upgrade pip
Установить зависимости из файла requirements.txt:

pip install -r requirements.txt
Перейдите в директорию, где лежит файл docker-compose.yml и запустит все контейнеры, описанные в конфиге:

docker compose up -d
Выполнить миграции:

docker compose exec backend python manage.py migrate
Собрать статику бэкенд-приложения:

sudo docker compose exec backend python manage.py collectstatic --no-input
Открыть в браузере страницу http://localhost:9000 - проверить корректность работы проекта.

Перейти по адресу http://localhost:9000/admin/ - убедиться, что страница отображается корректно.

Перейди в настройки репозитория проекта на GitHub — Settings -> Secrets and Variables → Actions -> New repository secret.

Сохранить переменные:

DOCKER_PASSWORD - пароль от аккаунта на Docker Hub;

DOCKER_USERNAME - имя пользователя Docker Hub;

HOST -  IP-адрес вашего сервера;

USER - имя пользователя на сервере;

SSH_KEY - содержимое текстового файла с закрытым SSH-ключом;

SSH_PASSPHRASE - парольная фраза к ssh-ключу;

TELEGRAM_TO - ID телеграм-аккаунта;

TELEGRAM_TOKEN - токен телеграм-бота.
Перейди в настройки репозитория проекта -> Variables → New repository variable.

Сохранить переменную:

SECRET_KEY - секретный ключ Django.
На удаленном сервере описать настройки Nginx-сервера:

sudo nano /etc/nginx/sites-enabled/default
server {
    server_name <ваш-домен>;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
Получить и настроить SSL-сертификат вашим любимым способом.

Проверить файл конфигурации на ошибки:

sudo nginx -t 
Перезагрузить конфигурацию Nginx:

sudo systemctl reload nginx 
На локальном компьютере в рабочей папке проекта проверить код на соответствие требованиям PEP8 по оформлению Python-кода:

(venv) ... Dev/kittygram_final$ flake8 .
Сделать коммит и запушить его в удалённый репозиторий:

(venv) ... Dev/kittygram_final$ git add .
(venv) ... Dev/kittygram_final$ git commit -m 'Add Actions'
(venv) ... Dev/kittygram_final$ git push
После получение сообщения об успешном размещение проекта, открыть в браузере страницу http://<ваш-домен> - проверить корректность работы проекта.

Перейти по адресу http://<ваш-домен>/admin/ - убедиться, что страница отображается корректно.

Автор
