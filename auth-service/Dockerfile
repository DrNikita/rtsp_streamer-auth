# Используем базовый образ для Go
FROM golang:1.21-alpine AS builder

# Устанавливаем зависимости
RUN apk update && apk add --no-cache git

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем файлы проекта в контейнер
COPY . .

# Загружаем зависимости и собираем проект
RUN go mod download
RUN go build -o auth-service .

# Создаем финальный минимальный образ для запуска
FROM alpine:3.18

# Устанавливаем зависимости, если требуются (например, для работы с базой данных)
RUN apk add --no-cache libc6-compat

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем бинарный файл из стадии сборки
COPY --from=builder /app/auth-service .

# Копируем файл с переменными окружения в контейнер
COPY .env .env

# Устанавливаем переменные окружения
ENV $(cat .env | xargs)

# Открываем порт, на котором работает auth-service (например, 8000)
EXPOSE 8000

# Запускаем приложение
CMD ["./auth-service"]
