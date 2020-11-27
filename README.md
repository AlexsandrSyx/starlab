[![Build Status](https://travis-ci.com/AlexsandrSyx/starlab.svg?branch=master)](https://travis-ci.com/AlexsandrSyx/starlab)
# Технология разработки программного обеспечения
## Лабораторная работа №1: создание микросервиса на Spring Boot с базой данных
# Суханов Александр Александрович, Группа 3MAC2001
# Цель лабораторной работы:
## Целью лабораторной работы является знакомство с проектированием многослойной архитектуры Web-API (веб-приложений, микро-сервисов).

Приложение представляет из себя простой микросервис, реализующий CRUD на примере продуктов. Для работы приложения требуется запущенная БД postgresql.

# Подготовка базы данных
В файле ./src/main/resources/application.properties следует указать в параметре spring.datasource.username = имя пользователя для доступа в БД, в параметре spring.datasource.password = пароль для доступа к БД. В параметре spring.datasource.url = необходимо указать адрес для доступа к БД, например для доступа к БД запущенной на локальном компьютере значение будет jdbc:postgresql://localhost:5432/postgresql, для БД запущенной в docker на локальной машине значение будет jdbc:postgresql://172.17.0.1:5432/postgresql.
Нужно запустить postgresql с помощью docker используя комманду 
- docker run -e POSTGRES_PASSWORD=root -p 5432:5432 postgres .
Настройка базы данных осуществляется с помощью ./src/main/resources/product.sql. 
Тестовые данные для БД находятся в ./src/main/resources/data.sql . 

# Клонирование репозитария
Для клонирования репозитория необходимо выполнить команду
- git clone https://github.com/AlexsandrSyx/starlabs.git
или же скачать zip-архив и распаковать его.

# Сборка проекта с помощью Maven
Сборка приложения осуществляется при помощи автоматической системы сборки проектов Maven. Для сборки необходимо выполнить команду mvn package -Dmaven.test.skip=true(с пропуском тестирования) находясь в директории проекта. После окончания выполнения команды появится папка target в которой находится скомпилированный код и файл simpleapi-1.0.jar.

# Сборка и запуск Docker-образа
Для сборки Docker образа следует выполнить команду docker build -t labapi:latest . находясь в директории с Dockerfile и собранным simpleapi-1.0.jar .
Запуск осуществляется командой docker run -p 8080:8080 labapi:latest , где первым указывается порт в локальной системе, а вторым порт docker.
#### Примеры запросов к simpleapi.
Формат JSON: {name: "varchar", brand: "varchar",price "integer", quantity: "integer"}
#### Получить список:
- curl -X GET http://localhost:8080/skynet/electronik/product В ответ будет получен JSON.
#### Получить запись по id:
- curl -X GET http://localhost:8080/skynet/electronik/product/{id} В ответ будет получен JSON с результатом.
#### Добавить запись:
- curl -X POST http://localhost:8080/skynet/electronik/product -d '{"name": "новый продукт", "brand": "новый бренд", "price": 100, "quantity": 100}' -H "Content-Type:application/json" в ответ будет получен JSON c новым product
#### Обновить запись:
- curl -X POST http://localhost:8080/skynet/electronik/product/{id} -d '{"name": "новый продукт", "brand": "новый бренд", "price": 100, "quantity": 100}' -H "Content-Type:application/json"
#### Удалить запись:
- curl -i -X DELETE http://localhost:8080/skynet/electronik/product/{id}
#### Также приложение возвращает значение hostname:
- curl -X GET http://localhost:8080/skynet/electronik/status В ответ будет получен JSON в виде {hostname: "hostname"}.
## Лабораторная работа №2: создание кластера Kubernetes и деплой приложения
Целью лабораторной работы является знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер.
#### Манифесты 
- deployment.yaml
>
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: my-app
      strategy:
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 1
        type: RollingUpdate
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
            - image: labapi:latest
              imagePullPolicy: Never 
              name: labapi
              ports:
                - containerPort: 8080
          hostAliases:
          - ip: "192.168.49.1"
            hostnames:
            - postgres.localhost

- service.yaml
>
    apiVersion: v1
    kind: Service
    metadata:
      name: my-service
    spec:
      type: NodePort
      ports:
        - nodePort: 31317
          port: 8080
          protocol: TCP
          targetPort: 8080
      selector:
        app: my-app

- #### Скриншоты вывода команды консоли с шага 3.3 на фоне рабочего стола.
![](https://github.com/AlexsandrSyx/starlab/blob/master/picture/Screen%20console%20command.png)
>Screen console command

- #### Скриншоты графического интерфейса с шага 3.5, где видны поды.
![](https://github.com/AlexsandrSyx/starlab/blob/master/picture/Screen%20graphical%20interface1.png)
>Screen graphical interface1

![](https://github.com/AlexsandrSyx/starlab/blob/master/picture/Screen%20graphical%20interface2.png)
>Screen graphical interface2

![](https://github.com/AlexsandrSyx/starlab/blob/master/picture/Screen%20graphical%20interface3.png)
>Screen graphical interface3
## Лабораторная работа №3: CI/CD и деплой приложения в Heroku
# Цель работы: 
### Целью лабораторной работы является знакомство с CI/CD и его реализацией на примере Travis CI и Heroku.

### Ссылка на развернутое приложение: 
- [получение status в приложении](https://labapi-mt.herokuapp.com/skynet/electronik/status)
- [получение всех product](https://labapi-mt.herokuapp.com/skynet/electronik/product/)
- [получение конекретного product](https://labapi-mt.herokuapp.com/skynet/electronik/product/{id})