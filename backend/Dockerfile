# ---------- Stage 1: Build ----------
FROM golang:1.22-alpine AS builder

WORKDIR /app

# Устанавливаем git (иногда нужен для go mod)
RUN apk add --no-cache git

# Копируем go.mod и go.sum отдельно (кэширование зависимостей)
COPY go.mod go.sum ./
RUN go mod download

# Копируем весь проект
COPY . .

# Собираем бинарник из cmd/api
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o app ./cmd/api

# ---------- Stage 2: Runtime ----------
FROM alpine:3.20

WORKDIR /app

RUN apk add --no-cache ca-certificates

# Копируем только бинарник
COPY --from=builder /app/app .

# Открываем порт (если в main.go используется 8080)
EXPOSE 8080

CMD ["./app"]