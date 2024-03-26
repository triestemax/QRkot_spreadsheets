# Кошачий благотворительный фонд QRKot

----------------------------------------
## Описание

Данный сервис создан для поддержки кошек. Пользователи могут внести пожертвование, 
сопроводив его комментарием или пожеланием. Администраторы сервиса создают проекты в помощь 
приютам для животных и другим заинтересованным. Проекты не целевые: распределение средств 
происходит автоматически в порядке создания проектов.

----------------------------------------
## Используемые технологии

 - Python 3.9
 - FastAPI (веб-фреймворк для создания API)
 - SQLAlchemy (библиотека для работы с реляционными СУБД с применением технологии ORM)
 - Pydantic (библиотека для валидации и сериализации данных)
 - Alembic (инструмент для миграции базы данных)
 - Uvicorn (высокопроизводительный ASGI сервер)
 
----------------------------------------
## Установка

1. Клонируйте репозиторий
```bash
git clone https://github.com/lmashik/cat_charity_fund.git
```

2. Создайте и активируйте виртуальное окружение
```bash
python3 -m venv env
```

* Если у вас Linux/macOS

    ```bash
    source env/bin/activate
    ```

* Если у вас windows

    ```bash
    source env/scripts/activate
    ```

3. Обновите pip до последней версии
```bash
python3 -m pip install --upgrade pip
```

4. Установите зависимости из файла requirements.txt
```bash
pip install -r requirements.txt
```

----------------------------------------
## Запуск

В директории проекта создайте файл .env и заполните его по образцу 
.env.example

Примените миграции
```bash
alembic upgrade head
```

Запустите проект
```bash
uvicorn app.main:app --reload
```

Для того чтобы открыть документацию проекта, перейдите по ссылке http://127.0.0.1:8000/docs/ после запуска проекта.

----------------------------------------
## API
Данный сервис является API, так что может быть интегрирован в вашу систему.

### Формат запроса
Запрос осуществляется посредством протокола HTTP 1.1.

### Формат ответа
Ответ сервиса представляет собой JSON-документ в кодировке UTF-8, 
содержимое зависит от запроса.

### Ресурсы
QRKot имеет три ресурса: Проекты, Пожертвования и Пользователи.
Работа с пользователями осуществляется с помощью стандартного модуля FastAPI Users.

Список проектов может быть просмотрен любым пользователем сервиса. Создание, редактирование 
и удаление проектов доступно только суперпользователям.

Пожертвование может сделать любой пользователь. Также он может посмотреть список 
своих пожертвований. Суперпользователь может посмотреть список всех пожертвований.

Для регистрации выполните POST запрос на http://127.0.0.1:8000/auth/register:
```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/auth/register' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "user@example.com",
  "password": "your_password"
  }'
```

Для аутентификации и получения токена выполните POST запрос на http://127.0.0.1:8000/auth/jwt/loginЖ
```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/auth/jwt/login' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=&username=username&password=password&scope=&client_id=&client_secret='
```

Пример ответа в случае успешного выполнения:
```json
{
  "access_token": "token",
  "token_type": "bearer"
}
```

Далее используйте этот токен при остальных запросах к сервису - передавайте его в заголовках запросов. 

**Пример**

Для создания нового пожертвования выполните POST запрос на http://127.0.0.1:8000/donation/,
пример которого ниже:
```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/donation/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "comment": "Помощь",
  "full_amount": 150
  }'
```

Пример ответа в случае успешного выполнения
_HTTP 1.1 200 OK_
```json
{
  "comment": "Помощь",
  "full_amount": 120,
  "id": 1,
  "create_date": "2024-10-19T03:21:26.369602"
}
```

----------------------------------------
### Автор проекта:
Максим Шабанов
[https://github.com/triestemax](https://github.com/triestemax)
