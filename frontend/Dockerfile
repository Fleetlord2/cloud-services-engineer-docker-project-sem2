# ---------- Stage 1: Build ----------
FROM node:16-alpine AS builder

WORKDIR /app

ARG VUE_APP_API_URL
ENV VUE_APP_API_URL=$VUE_APP_API_URL

COPY package*.json ./
RUN npm ci

COPY . .

RUN npm run build


# ---------- Stage 2: Runtime ----------
FROM nginx:1.27-alpine

RUN rm /etc/nginx/conf.d/default.conf

COPY --from=builder /app/dist /usr/share/nginx/html

RUN printf 'server { \
    listen 80; \
    server_name _; \
    location /momo-store/ { \
        alias /usr/share/nginx/html/; \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]