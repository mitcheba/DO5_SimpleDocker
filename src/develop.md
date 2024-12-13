# Simple Docker

## Part 1. Готовый докер

Взять официальный Docker образ от nginx и загрузить `docker pull`

Проверить наличие образа Docker c помощью `docker images`

Запустить образ Docker с помощью `docker run -d nginx`

Поменял название контейнера для удобства.

Проверить, что образ запущен с `docker ps`

![Part1.1](screenshot/part1/part1.1.png)

Из вывода команды `docker inspect` определить и записать в отчет размер контейнера, список сопоставленных портов и IP-адреса контейнера

![Part1.2](screenshot/part1/part1.2.png)

![Part1.3](screenshot/part1/part1.3.png)

![Part1.4](screenshot/part1/part1.4.png)

Остановить образ Docker с помошью `docker stop [container_name]`

Убедиться, что изображение остановлено с помощью `docker ps`

Запустить Docker с портами 80 и 443 в контейнере, сопоставленным с теми же портами на локальной машине, с помощью команды `run`

![Part1.5](screenshot/part1/part1.5.png)

Проверить, доступна ли стартовая страница nginx в бразуере по адресу localhost:80

![Part1.6](screenshot/part1/part1.6.png)

Перезапустить Docker-контейнер с помощью `docker restart [container_name]`

Проверить любым способом, работоспособность контейнера

![Part1.7](screenshot/part1/part1.7.png)

(После запуска Dockera с портами 80 и 443, создался новый контейнер, переименовал)

## Part 2. Операции с контейнером

Прочитать конфигурационный файл `nginx.conf` внутри докер контейнера через команду `exec`

![Part2.1](screenshot/part2/part2.1.png)

Создать на локальной машине файл `nginx.conf`

![Part2.2](screenshot/part2/part2.2.png)

Настроить в нем по пути `/status` отдачу страницы статуса сервера `nginx`

![Part2.3](screenshot/part2/part2.3.png)

Скопировать созданный файл `nginx.conf` внутрь докер-образа через команду `docker cp`.

Перезапусти `nginx` внутри докер-образа через команду `exec`.

![Part2.4](screenshot/part2/part2.4.png)

Проверить, что по адресу `localhost:80/status` отдается страничка со статусом сервера `nginx`

![Part2.5](screenshot/part2/part2.5.png)

Экспортировать контейнер в файл `container.tar` через команду `export`.

Остановить контейнер.

Удали образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры.

Удалить остановленный контейнер.

![Part2.6](screenshot/part2/part2.6.png)

Импортировать контейнер обратно через команду `import`.

Запустить импортированный контейнер.

Проверить, что по адресу `localhost:80/status` отдается страничка со статусом сервера `nginx`.

![Part2.8](screenshot/part2/part2.7.png)

### Part 3. Мини веб-сервер

Напиcать мини-сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью `Hello World!`.

![Part3.1](screenshot/part3/part3.1.png)

Запукая Docker на 81 порту.

![Part3.2](screenshot/part3/part3.2.png)

Запустить написанный мини-сервер через  `spawn-fcgi` на порту 8080.

![Part3.5](screenshot/part3/part3.5.png)

Написать свой `nginx.conf`, который будет проксировать все запросы с 81 порта на 127.0.0.1:8080.

![Part3.3](screenshot/part3/part3.3.png)

Копирую файл в внуть docker-образа и перезапустил 

![Part3.4](screenshot/part3/part3.4.png)

Проверить, что в браузере по `localhost:81` отдается написанная тобой страничка.

![Part3.6](screenshot/part3/part3.6.png)

Положи файл nginx.conf по пути ./nginx/nginx.conf (выше).


### Part 4. Свой докер
 
Напиши свой докер-образ, который:

1) собирает исходники мини сервера на FastCgi из Части 3;

2) запускает его на 8080 порту;

3) копирует внутрь образа написанный ./nginx/nginx.conf;

4) запускает nginx

![Part4.1](screenshot/part4/part4.1.png)

Скрипт в докер образе

![Part4.2](screenshot/part4/part4.2.png)

Собери написанный докер-образ через docker build при этом указав имя и тег.

Проверь через docker images, что все собралось корректно.

![Part4.3](screenshot/part4/part4.3.png)

Проверь, что по localhost:80 доступна страничка написанного мини сервера.

![Part4.4](screenshot/part4/part4.4.png)

Допиши в ./nginx/nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx.

Перезапусти докер-образ.

![Part4.5](screenshot/part4/part4.5.png)

Проверь, что теперь по localhost:80/status отдается страничка со статусом nginx

![Part4.6](screenshot/part4/part4.6.png)

### Part 5. Dockle

Просканировать образ из предыдущего задания через dockle `image_id|repository`.

![Part5.1](screenshot/part5/part5.1.png)

Исправить образ так, чтобы при проверке через dockle не было ошибок и предупреждений.

![Part5.2](screenshot/part5/part5.2.png)

### Part 6. Базовый Docker Compose

Написать файл `docker-compose.yml`, с помощью которого:

1) Поднять докер-контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину).

2) Поднять докер-контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера.

Замапить 8080 порт второго контейнера на 80 порт локальной машины.

![Part6.1](screenshot/part6/part6.1.png)

Остановить все запущенные контейнеры.

Соберать и запустить проект с помощью команд `docker-compose build` и `docker-compose up`.

Проверить, что в браузере по localhost:80 отдается написанная тобой страничка, как и ранее.

![Part6.2](screenshot/part6/part6.2.png)
