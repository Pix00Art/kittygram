# Kittygram  [![Kittygram](https://github.com/Pix00Art/kittygram_final/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/Pix00Art/kittygram_final/actions/workflows/main.yml)

## Kittygram - это веб-приложение, в котором можно:

- [x] Регистрировать аккаунты пользователей.

- [x] Добавлять информацию о питомцах: имя, год рождения, фотографии, достижения, цвет.

- [x] Просматривать и редактировать данные питомцев.

---
## Cтек:

- [x] **Frontend**: `Node.js`  `React`

- [x] **Backend**: `Python` `Django` `Django Rest Framework` `Gunicorn` `Pillow`

- [x] **Database**: `PostgreSQL`

- [x] **Infrastructure**: `Nginx` `Docker` `GitHub Actions`

---
## Установка и запуск проекта локально:

- [x] **Клонируйте репозиторий:**
```
  git clone https://github.com/Pix00Art/kittygram_final.git
```
- [x] **Перейдите в директорию проекта и создайте файл .env:**
```
   cd kittygram_final && touch .env
```

- [x] **Заполните переменные окружения в файле .env:**
```  
POSTGRES_DB=<Название Базы Данных>
POSTGRES_USER=<Имя Пользователя БД>
POSTGRES_PASSWORD=<Пароль БД>
DB_HOST=db
DB_PORT=5432

DEBUG = <True/False>
SECRET_KEY = <секретный ключ Django>
ALLOWED_HOSTS = <127.0.0.1,localhost,server_ip_address,your_domain_address>
SQLITE_ENGINE = <True/False> (если хотите использовать SQLite вместо PostgreSQL укажите True)
DOCKER_USERNAME = ваш никнейм на DockerHub
```

- [x] **Установите Docker и Docker Compose c официального сайта под вашу операционную систему [Docker](https://docs.docker.com/get-started/get-docker/):**
- [x] **Соберите образы и залейте на свой [DockerHub](https://hub.docker.com/):**
    
```
cd frontend
docker build -t your_username/kittygram_frontend . && docker push username/kittygram_frontend
cd ../backend
docker build -t your_username/kittygram_backend .  && docker push username/kittygram_backend
cd ../nginx
docker build -t your_username/kittygram_gateway .  && docker push username/kittygram_gateway


```
- [x] **Выпоните команду Docker Compose:**
```
docker compose -f docker-compose.production.yml up
```
- [x] **Сервис будет доступен по ссылке:**
```
http://localhost:9000/
```

---

## Установка и запуск проекта на сервере Ubuntu:
- [x] **Подключитесь к удаленному серверу:**
```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
```
- [x] **Установите Docker, Docker Compose, Nginx на сервер:**
```
sudo apt update
sudo apt install nginx -y
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```
- [x] **В директорию kittygram скопируйте файлы docker-compose.production.yml и .env:**
```
mkdir kittygram
scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram_final/docker-compose.production.yml
scp -i path_to_SSH/SSH_name .env username@server_ip:/home/username/kittygram_final/.env
```
- [x] **На сервере отредактируйте конфигурацию Nginx:**
```
sudo nano /etc/nginx/sites-enabled/default
```
```
server {
    server_name your_domain_address;
    server_tokens off;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
        client_max_body_size 20M;
    }
}
```
```
sudo nginx -t
sudo service nginx reload
```
- [x] **Запустите docker compose в режиме демона:**
```
sudo docker compose -f docker-compose.production.yml up -d
```
- [x] **Выполните миграции, соберите статические файлы бэкенда и скопируйте их в /backend_static/static/:**
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
- [x] **Сервис будет доступен по ссылке:**
```
http://your_domain_address/
```

---

## Автоматический деплой проекта на сервер:

- [x] После внесения правок в проект на Github:
   - GitHub Actions выполнит все необходимые команды и тесты описанные в файле .github/workflows/main.yml
   - Проект автоматически запуститься на сервере, телеграмм бот отправит сообщение c информацией о деплое. 

- [x] Для запуска workflow необходимо добавить в Secrets репозитория на GitHub переменные окружения:
```
POSTGRES_DB=<название базы данных>
POSTGRES_USER=<Имя Пользователя БД>
POSTGRES_PASSWORD=<Пароль БД>

SECRET_KEY=<Секретный ключ Django>

HOST=<ip сервера>
USER=<Имя Пользователя для подключения к удаленному серверу>
SSH_KEY=<Приватный SSH-ключ>
SSH_PASSPHRASE=<Фраза-пароль для сервера>

TELEGRAM_TO=<id Телеграм аккаунта>
TELEGRAM_TOKEN=<токен Телеграм бота>

DOCKER_USERNAME=<имя пользователя DockerHub>
DOCKER_PASSWORD=<пароль от DockerHub>


```


---
## **Автор:**

**Artem Gorbunov**

[![Artem Gorbunov](https://img.shields.io/badge/Github-Artem.Gorbunov-red)](https://github.com/Pix00Art)