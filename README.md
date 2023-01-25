##### Тестовое задание на вакансию Backend-developer
- Python 3.9.13, Django 4.1.5, Celery 5.2.7, Redis 4.4.2
- База данных SQLite, поскольку в ТЗ указано использовать любую и проект не деплоится на сервер. При возможном размещении на сервере я бы подключил  PostgreSQL.
- Для размещения секретных ключей используется стандартный файл .env, дополнительный файл .env используется для сохранения токена юзера (для получения баланса), чтобы не рисковать основными секретными данными.
- Для периодических задач используется Celery + Redis (запускается в Docker: `docker run -d -p 6379:6379 redis` )
- Для запуска периодических задач запускаем сервер Redis через Docker, далее в терминале из директории с **manage.py** запускаем обработчик и планировщик задач командами: `celery -A mera_capital worker -l info --pool=solo` и `celery -A mera_capital beat -l info`
- Посмотреть периодические задачи можно с помощью Flower по адресу http://127.0.0.1:5566, запустив из директории с **manage.py**: `celery -A mera_capital flower  --address=127.0.0.1 --port=5566`

Структура файлов и директорий:

    m_capital_test_task_backend/
    ├── mera_capital/
    │   ├── mera_capital/
    │   │   ├── ...
    │   │   ├── celery.py --> настройки периодических задач
    │   │   └── ...
    │   ├── pnls/
    │   │   ├── ...
    │   │   ├── services.py --> получение информации через API
    │   │   ├── tasks.py --> периодические задачи
    │   │   └── ...
    │   ├── static/ --> файлы статики
    │   ├── templates/ --> html шаблоны
    │   ├── .env.example --> токен для получения баланса
    │   ├── db.sqlite3
    │   └── manage.py
    ├── .env.example --> секретные ключи
    ├── README.md
    └── requirements.txt``
