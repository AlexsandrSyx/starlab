# Технология разработки программного обеспечения
## Лабораторная работа №1: создание микросервиса на Spring Boot с базой данных
# Суханов Александр Александрович, Группа 3MAC2001
# Цель лабораторной работы:
## Целью лабораторной работы является знакомство с проектированием многослойной архитектуры Web-API (веб-приложений, микро-сервисов).

Приложение представляет из себя простой микросервис, реализующий CRUD на примере внутренней телефонной базы. Для работы приложения требуется запущенная БД postgresql.

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

# Примеры запросов к simpleapi.
Формат JSON: {name: "varchar", brand: "varchar",price "integer", quantity: "integer"}

# Получить список:
curl -X GET http://127.0.0.1:8080/skynet/electronik/product В ответ будет получен JSON.

# Получить запись по id:
curl -X GET http://127.0.0.1:8080/skynet/electronik/product{id} В ответ будет получен JSON с результатом.

# Добавить запись:
curl -X POST http://127.0.0.1:8080/skynet/electronik/product -d ‘{«name»: "новое продукт″, "brand": "новый бренд", "price": 10000, "quantity": 1000}’ -H «Content-Type:application/json» В ответ будет получен статус 200 ОК.

# Удалить запись:
curl -i -X DELETE http://127.0.0.1:8080/skynet/electronik/product/del/{id} В ответ будет получен статус 204 No Content.

#Также приложение возвращает значение hostname:
curl -X GET http://127.0.0.1:8080/skynet/electronik/status В ответ будет получен JSON в виде {hostname: "hostname"}.