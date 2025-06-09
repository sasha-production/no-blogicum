# Базовый образ Python для Django
FROM python:3.9-slim AS base

# Установка зависимостей для Django и PostgreSQL
RUN apt-get update && apt-get install -y \
    libpq-dev \
    gcc \
    nginx \
    && rm -rf /var/lib/apt/lists/*

# Установка переменных окружения
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Установка рабочей директории
WORKDIR /app

# Копируем зависимости и устанавливаем их
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Копируем проект
COPY . .

# Настройка Nginx
RUN rm /etc/nginx/sites-enabled/default
COPY nginx.conf /etc/nginx/conf.d/

# Создаем директорию для сокета Gunicorn
RUN mkdir -p /run/gunicorn

# Указываем порты
EXPOSE 80

# Запуск Nginx и Gunicorn
CMD service nginx start && gunicorn --bind unix:/run/gunicorn/gunicorn.sock blogicum.wsgi:application
