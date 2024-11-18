# Указываем базовый образ для Go
FROM golang:1.21-alpine AS builder

# Устанавливаем зависимости
RUN apk update && apk add --no-cache git

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем все файлы проекта в контейнер
COPY . .

# Устанавливаем зависимости и собираем бинарный файл
RUN go mod download
RUN go build -o rtsp-streamer .

# Создаем финальный минимальный образ для запуска
FROM alpine:3.18

# Устанавливаем зависимости для работы с RTSP и WebRTC (например, ffmpeg)
RUN apk add --no-cache ffmpeg

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем бинарный файл из стадии сборки
COPY --from=builder /app/rtsp-streamer .

# Копируем файл с переменными окружения в контейнер
COPY .env .env

# Указываем переменные окружения
ENV $(cat .env | xargs)

# Открываем необходимые порты (например, 8080 для HTTP и 8554 для RTSP, если они используются)
EXPOSE 8080
EXPOSE 8554

# Указываем команду для запуска приложения
CMD ["./rtsp-streamer"]