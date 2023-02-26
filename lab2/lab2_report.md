### University: [ITMO University](https://itmo.ru/ru/)
### Faculty: [FICT](https://fict.itmo.ru)
### Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
### Year: 2022/2023
### Group: K4111c
### Author: Sokolova Anna Aleksandrovna
### Lab: Lab2
### Date of create: 27.11.2022
### Date of finished: 


# Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."
## Цель работы
Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть веб приложение.
## Ход работы
## Cоздание deployment
Написание манифеста для создания deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передача переменных в эти реплики: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: ifilyaninitmo/itdt-contained-frontend:master
        env:
        - name: REACT_APP_USERNAME
          value: Anna
        - name: REACT_APP_COMPANY_NAME
          value: ITMO_University
        ports:
        - containerPort: 3000
```
Применение манифеста и проверка успешного создания deployment и pods.
```
kubectl apply -f deployment.yml
```
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/2d23e2f0ed44dc548e5aa3c63e552a354602d8d6/lab2/images/applyingthemanifest.png)
## Создание сервиса, через который будет доступ на pods.
```
kubectl expose deployment frontend --port=3000 --type=LoadBalancer
```
## Запуск в minikube режима проброса портов и подключение к контейнерам через веб-браузер.
```
minikube kubectl -- port-forward deployment/frontend 3000:3000
```
## Проверка на странице в веб-браузере переменных REACT_APP_USERNAME, REACT_APP_COMPANY_NAME и Container name.
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/2d23e2f0ed44dc548e5aa3c63e552a354602d8d6/lab2/images/deployment.png)

После перезагрузки ничего не изменилось, переменные остались теми же. Похоже, что нагрузка небольшая, поэтому запросы не пересылаются на другой "под".
## Проверка логов контейнеров.
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/2d23e2f0ed44dc548e5aa3c63e552a354602d8d6/lab2/images/logs.png)
## Создание схемы организации контейнеров и сервисов

