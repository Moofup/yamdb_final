# API YaMDb

## _API интернет-сервиса YaMDB для хранения рецензий на произведения_

![workflow](https://github.com/moofup/yamdb_final/actions/workflows/yamdb_workflow/badge.svg)

### Описание проекта API YaMDB

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title). 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
Список категорий (Category) и жанров (Genre) может быть расширен.
Отзывы могут комментироваться пользователями (Comments).

### Стек 
* Python 3.7
* Django
* DRF
* Simple-JWT
* PostgreSQL
* Docker
* nginx
* gunicorn

### Пользовательские роли

- __Аноним__ — может просматривать описания произведений, читать отзывы 
и комментарии.
- __Аутентифицированный пользователь (user)__ — может читать всё, как и Аноним,
может публиковать отзывы и ставить оценки произведениям, может комментировать 
отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать 
свои оценки произведений. Эта роль присваивается по умолчанию каждому новому 
пользователю.
- __Модератор (moderator)__ — те же права, что и у Аутентифицированного 
пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- __Администратор (admin)__ — полные права на управление всем контентом 
проекта. Может создавать и удалять произведения, категории и жанры. 
Может назначать роли пользователям.
- __Суперюзер Django__ должен всегда обладать правами администратора, 
пользователя с правами admin. Даже если изменить пользовательскую роль 
суперюзера — это не лишит его прав администратора. Суперюзер — всегда 
администратор, но администратор — не обязательно суперюзер.

### Запуск приложения в контейнерах

Клонировать репозиторий и перейти в корневую папку:
```
git clone git@github.com:Moofup/infra_sp2.git
cd infra_sp2
```

Перейти в папку infra_sp2/infra и создать в ней файл .env с 
переменными окружения, необходимыми для работы приложения.
```
cd infra/
```

Пример содержимого файла .env:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=key
```

Запустить docker-compose: 
```
docker-compose up -d
```

Внутри контейнера web выполнить миграции, создать 
суперпользователя и собрать статику:
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```
После этого проект будет доступен по адресу http://localhost/. 

### Заполнение базы данных

Зайти на http://localhost/admin/, авторизоваться и внести записи 
в базу данных через админку.

Резервную копию базы данных можно создать командой
```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```

### Остановка контейнеров

Открыть второй терминал и воспользоваться командой
```
docker-compose stop 
```

Запустить контейнеры, которые уже были созданы ранее, без их повторного создания
```
docker-compose start 
```

### Остановка и удаление контейнеров и созданных томов

Открыть второй терминал и воспользоваться командой
```
docker-compose down -v
```

### Документация в формате Redoc:

Локально запустить проект и перейти на страницу 
http://127.0.0.1:8000/redoc/
