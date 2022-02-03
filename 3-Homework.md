
# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---

## Задача 1

Сценарий выполения задачи:

- создайте свой репозиторий на https://hub.docker.com;
- выберете любой образ, который содержит веб-сервер Nginx;
- создайте свой fork образа;
- реализуйте функциональность:
запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
```   
 
###### Данную задачу попытался сделать в вирт.машине на убунту. Вывод команд ниже - для того чтоб можно было найти ошибки, если будут.  
###### vagrant@vagrant:~$ vim Dockerfile
###### vagrant@vagrant:~$ vim test1.html
###### vagrant@vagrant:~$ ls -l
###### total 8
###### -rw-rw-r-- 1 vagrant vagrant 63 Feb  3 16:19 Dockerfile
###### -rw-rw-r-- 1 vagrant vagrant 99 Feb  3 16:20 test1.html
###### vagrant@vagrant:~$ docker build . -t test2
###### Sending build context to Docker daemon  19.97kB
###### Step 1/3 : FROM nginx:1.21.5
###### ---> 605c77e624dd
###### Step 2/3 : COPY test1.html /usr/share/nginx/html
###### ---> e7273b1b54b0
###### Step 3/3 : EXPOSE 80
###### ---> Running in b1b98803086c
###### Removing intermediate container b1b98803086c
###### ---> b9154eec2575
###### Successfully built b9154eec2575
###### Successfully tagged test2:latest
###### vagrant@vagrant:~$ docker run --name test2 -d -p 8080:80 test2
###### e8f63abc7af4b6b4e2d1ed82cb1e3a708273ed0398028a608b4000f77e3b6541
###### vagrant@vagrant:~$ docker login
###### Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
###### Username: marsfromperm
###### Password:
###### WARNING! Your password will be stored unencrypted in /home/vagrant/.docker/config.json.
###### Configure a credential helper to remove this warning. See
###### https://docs.docker.com/engine/reference/commandline/login/#credentials-store
###### 
###### Login Succeeded
###### vagrant@vagrant:~$ docker tag test2:latest marsfromperm/netology:0.1
###### vagrant@vagrant:~$ docker push marsfromperm/netology:0.1
###### The push refers to repository [docker.io/marsfromperm/netology]
###### 6e401cc41374: Pushed
###### d874fd2bc83b: Mounted from library/nginx
###### 32ce5f6a5106: Mounted from library/nginx
###### f1db227348d0: Mounted from library/nginx
###### b8d6e692a25e: Mounted from library/nginx
###### e379e8aedd4d: Mounted from library/nginx
###### 2edcec3590a4: Mounted from library/nginx
###### 0.1: digest: sha256:68f3a2c16c2ed898f870e3f41f8f24bcdd40cae7080d153f34718dfd6cdf281f size: 1777
###### vagrant@vagrant:~$  
###### vagrant@vagrant:~$ ls -l
###### total 8
###### -rw-rw-r-- 1 vagrant vagrant 68 Feb  3 16:25 Dockerfile
###### -rw-rw-r-- 1 vagrant vagrant 99 Feb  3 16:20 test1.html
###### vagrant@vagrant:~$ cat Dockerfile
###### FROM nginx:1.21.5
###### 
###### COPY test1.html /usr/share/nginx/html
###### 
###### EXPOSE 80
###### vagrant@vagrant:~$ cat test1.html
###### <html>
######         <head>
######                 Hey, Netology
######         </head>
######         <body>
######                 <h1>I’m DevOps Engineer!</h1>
######         </body>
###### </html>
###### vagrant@vagrant:~$  

## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение;  
###### Физический сервер. Так как монолитное приложение, а не россыпь микросервисов. Запуск монолита в контейнере нам выигрыша не дает.  
- Nodejs веб-приложение;  
###### В данном случае - Docker. Присутсвует возможность масштабирования приложения.  
- Мобильное приложение c версиями для Android и iOS;  
###### В данном случае - Docker. Присутствует возможность упаковать все необходимые данные для последующей распаковки.  
- Шина данных на базе Apache Kafka;  
###### В данном случае - Docker. За счет идемпотентности.   
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;  
###### В данном случае - Docker. Присутствует возможность упаковать все необходимые данные для последующей распаковки.  
- Мониторинг-стек на базе Prometheus и Grafana;  
###### В данном случае - Docker. Легко поднимается в любом месте и также легко мониторится в случае в докером.  
- MongoDB, как основное хранилище данных для java-приложения;  
###### В данном случае - Docker. Присутствует возможность упаковать все необходимые данные для последующей распаковки.  
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.  
###### Физический сервер. Основное требование - непрерывная работа, все данные должны быть в постоянном доступе.  

## Задача 3

- Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```;
- Добавьте еще один файл в папку ```/data``` на хостовой машине;
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.


---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---