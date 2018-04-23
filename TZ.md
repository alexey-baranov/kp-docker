# Простота
Одна из главных задач - сделать развертывание системы очень простым. Наши единомышленники должны иметь возможность без специальных знаний поднять точную копию kopnik.org. Чем проще это будет реализовано, тем большее количество программистов сможет реализовать свое желание и принять участие в разработке. От того какое количество программистов сможет принять участие зависит скорость, с которой мы пойдем впереди и успех всего дела. Если есть еще дополнительные технологии, которые помогают упростить развертывание, их тоже нужно включать в это ТЗ и постоянно его совершенствовать в этом направлении.

# Docker контейнеры
Ниже идут докер контейнеры. Все связи между контейнерами прописаны в общем compose- файле. Всe контейнеры поднимаются при помощи **docker stack deploy**. 
## Postgres
https://docs.docker.com/engine/examples/postgresql_service/
https://github.com/docker-library/docs/tree/master/postgres
- Последняя LTS 
- Стандартный порт торчит из контейнера  и виден в контейнере nodejs (сервер kopnik.org)
- max_connections = 1000
- shared_buffers = 512MB
- effective_cache_size = 1024MB
- work_mem = 40MB
## Почтовик
- Входящие порты закрыты, сервер только отправляет сообщения
- Стандартный порт для отправки торчит из контейнера и виден в контейнере nodejs (сервер kopnik.org)
## nodejs (сервер kopnik.org)
https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
https://github.com/nodejs/docker-node/blob/master/docs/BestPractices.md
- Исходный код https://github.com/alexey-baranov/kp-server установить в папку /home/ubuntu/sftp/kp-server и подключить внутрь контейнера через volume 
- Установить зависимости (команда **npm install** из папки с кодом)
- Внутри контейнера доступны порты для отправки почты 25, стандартынй порт Postgres, порт Crossbar 8080
- Команда для запуска контейнера **npm run server:production**
## nginx
https://docs.docker.com/samples/library/nginx/
- Принимает входящие по 80 и 443 
- Плагин для HTTP 2.0
- Три маршрута /ws, /api и по умолчанию
- маршрут /api перенаправляет весь трафик на порт 80 контейнера node.js (сервер kopnik.org)
- маршрут по умолнчанию отдает статические файлы из папки /home/ubuntu/kp-client/dist
- Сертификаты letsencrypt на хосте подключены внутрь контейнера через volume
## visualizer
https://github.com/dockersamples/docker-swarm-visualizer