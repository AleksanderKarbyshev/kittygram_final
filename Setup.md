## Развёртывание проекта:
+ Клонируйте репозиторий и перейдите в него:
```
git clone git@github.com:AleksanderKarbyshev/kittygram_final.git
```

```
cd kittygram_final/
```

+ Cоздайте и активируйте виртуальное окружение (Windows/Bash):
```
python -m venv venv
```

```
source venv/Scripts/activate
```

+ Установите зависимости из файла requirements.txt:
```
pip install -r backend/requirements.txt
```

+ Установите [Docker compose](https://www.docker.com/) на свой компьютер.

+ Запустите проект через docker-compose:
```
docker compose -f docker-compose.production.yml up --build -d
```

+ Выполнить миграции:
```
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```

+ Соберите статику:
```
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
```

+ Скопируйте статику:
```
docker compose -f docker-compose.production.yml exec backend cp -r /app/static_backend/. /backend_static/static/
```

+ Создать файл `.env` с переменными окружениями:

[Переменные окружения](./.env.example)

<br>

## Workflows:

CI и CD реализовано при помощи `GitHub Actions` 

Для работы проекта необходимо прописать переменные окружения в настройках репозитория на `GitHub`:

```txt
# имя пользователя в Docker
NICKNAME

# имя пользователя в DockerHub
DOCKER_USERNAME    

# пароль пользователя в DockerHub
DOCKER_PASSWORD    

# ip адрес вашего сервера
SSH_HOST      

# имя пользователя на вашем сервере                    
SSH_USER    

# приватный ssh-ключ вашего сервера
SSH_KEY    

# пароль для ssh-ключа ваше сервера            
SSH_PASSPHRASE                   

# id вашего телеграм-аккаунта ( узнать его можно при помоще бота @userinfobot)
TELEGRAM_TO      

# токен вышего бота (узнать (создать) его можно при помощи бота @BotFather)
TELEGRAM_TOKEN                 
```

При `push` автоматически отрабатывают сценарии:

+ `Tests 3.9` - проверка кода на соответствие стандарта PEP8;
+ `Push Docker image to DockerHub`, `Push gateway Docker image to DockerHub`, `Push frontend Docker image to DockerHub` - сборка и доставка докер-образов на DockerHub;
+ `Deploy` - автоматический деплой проекта на ваш сервер;
+ `Send message` - отправка уведомления в Telegram.

<br>

## Тестирование проекта:
Тестирование реализовано с использованием библиотеки Pytest. 

+ Запустить тесты из основной директории проекта:
```
pytest
```
