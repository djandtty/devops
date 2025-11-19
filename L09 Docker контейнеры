1. Завести репозиторий в Docker Hub.  
Готово  
2. Упаковать ваше микросервисное приложение в Dockerfile
Готово  
Dockerfile для backend (Node.js + Express):
```
FROM node:20-alpine

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Dockerfile для frontend:
```
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:1.27-alpine

COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
Конфиг nginx.conf:
```
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location /api/ {
        proxy_pass http://backend:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```
3. Собрать образ из полученного Dockerfile  
Готово  
```
docker build -t my-frontend:latest .
docker rm -f frontend
docker run -d \
  --name frontend \
  --network app-net \
  -p 80:80 \
  my-frontend:latest
```
docker build -t my-backend:latest .

docker run -d \
  --name backend \
  --network app-net \
  -p 3000:3000 \
  -e DB_USER=myuser \
  -e DB_PASSWORD=mypassword \
  -e DB_HOST=db \
  -e DB_NAME=mydb \
  -e DB_PORT=5432 \
  my-backend:latest
```

```
4. Запушить в репозиторий свои образы из предыдущего шага.  
Готово  
```
docker login
docker tag
docker push
```

5. Настроить хранение данных вашего приложения/бд в volume.  
Готово
```
docker volume create pgdata
docker volume ls
docker run -d \
  --name db \
  --network app-net \
  -e POSTGRES_DB=mydb \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=mypassword \
  -v pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:16-alpine
```

6. Создать отдельную Docker-network для вашего стэка приложения.  
Готово  
`docker network create app-net`  

7. Запустить ваше приложение в контейнерах в Yandex Cloud.  
Готово  
```
docker login
docker pull
```
8. Предоставить доступ к ВМ, для проверки доступности образов.  
???  
9. Дополнительное задание (не обязательное) - Написать скрипт для бэкапирования volumes.  
Готово
```
#!/bin/bash

VOLUME_NAME="pgdata"
BACKUP_DIR="/opt/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="${BACKUP_DIR}/${VOLUME_NAME}_${TIMESTAMP}.tar.gz"

mkdir -p "${BACKUP_DIR}"

docker run --rm \
  -v ${VOLUME_NAME}:/volume \
  -v ${BACKUP_DIR}:/backup \
  alpine \
  sh -c "tar czf /backup/$(basename ${BACKUP_FILE}) -C /volume ."
```




