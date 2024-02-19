#  Проект Kittygram.

## Описание проекта

Проект Kittygram - это сервис для любителей котиков, который позволяет пользователям размещать фотографии своих любимцев, информацию о них и их достижениях, а также смотреть фотографии других котиков.


## Как развернуть проект на своем сервере
1. Установить веб-сервер nginx
2. На сервере создать папку `kittygram`
3. В папку скопировать файл [docker-compose.production.yml](https://github.com/dodonova/kittygram_final/blob/main/docker-compose.production.yml)
4. В этой же папке создать файл `.env` в котором разместить информацию о переменных окружения. Пример в файле [.env.example](https://github.com/dodonova/kittygram_final/blob/main/.env.example)
5. Отредактировать конфигурацию `nginx sudo nano /etc/nginx/sites-enabled/default` :
```
server { 
  server_name IP-адрес-сервера ваше-доменное-имя; 
  location / { 
    proxy_pass http://127.0.0.1:9000; 
  }
}
```
6. Проверить конфигурацию nginx: 
```sudo nginx -t
```
7. Запустить Docker Compose на сервере в папке в проектом: 
```
sudo docker compose -f docker-compose.production.yml up -d
```
8. Выполнить миграции: 
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```
9. Собрать и скопировать статику: 
 ```
 sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic 
 sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/ 
 ```
10. Перезапустить nginx: 
```
sudo systemctl restart nginx
``` 
