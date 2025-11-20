docker-compose.yml:  
```
services:
  db:
    image: db:latest
    container_name: db
    restart: unless-stopped
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-net
    ports:
      - "5432:5432"

  backend:
    image: backend:latest
    container_name: backend
    restart: unless-stopped
    environment:
      DB_HOST: db
      DB_NAME: mydb
      DB_USER: myuser
      DB_PASSWORD: mypassword
      DB_PORT: 5432
    networks:
      - app-net
    ports:
      - "3000:3000"
    depends_on:
      - db

  frontend:
    image: frontend:latest
    container_name: frontend
    restart: unless-stopped
    networks:
      - app-net
    ports:
      - "80:80"
    depends_on:
      - backend

volumes:
  pgdata:

networks:
  app-net:
    driver: bridge

```
Запуск docker-compose:  
`docker-compose up -d`  
Сначала нужно docker pull этих контейнеров и привести к нужному имени  

