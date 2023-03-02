### University: [ITMO University](https://itmo.ru/ru/)
### Faculty: [FICT](https://fict.itmo.ru)
### Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
### Year: 2022/2023
### Group: K4111c
### Author: Sokolova Anna Aleksandrovna
### Lab: Lab4
### Date of create: 27.11.2022
### Date of finished: 


# Лабораторная работа №4 "Сети связи в Minikube, CNI и CoreDNS"
## Цель работы
Познакомиться с CNI Calico и функцией IPAM Plugin, изучить особенности работы CNI и CoreDNS.
## Ход работы
## При запуске minikube установка плагин CNI=calico и режима работы Multi-Node Clusters одновеременно. В рамках данной лабораторной работы развернуто 2 ноды.
```
minikube start --nodes=2 --cni=calico
```

## Проверка работы CNI плагина Calico и количества нод.

![Image text]()

![Image text]()

## Проверка работы Calico с помощью функции IPAM Plugin.
Для проверки режима IPAM необходимо для запущеных ранее нод указать label по признаку стойки или географического расположения

![Image text]()

## Разработка манифеста для Calico, который на основе ранее указанных меток назначает бы IP адреса "подам", исходя из пулов IP адресов которые указаны в манифесте.

![Image text]()

```
kubectl calico create --allow-version-mismatch -f pool1.yml
kubectl calico create --allow-version-mismatch -f pool2.yml
```
![Image text]()

## Создание deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передача переменных в эти реплики: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lab4
  labels:
    app: lab4
spec:
  replicas: 2
  selector:
    matchLabels:
      app: lab4
  template:
    metadata:
      labels:
        app: lab4
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

```
kubectl apply -f deployment.yml
```

## Создать сервис через который у вас будет доступ на эти "поды"

```
kubectl expose deployment lab4 --port=3000 --type=LoadBalancer
```

## Запустить в minikube режим проброса портов и подключитесь к вашим контейнерам через веб браузер

```
minikube kubectl -- port-forward deployment/lab4 3000:3000
```

## Проверка на странице в веб-браузере переменных REACT_APP_USERNAME, REACT_APP_COMPANY_NAME и Container name

![Image text]()

## 

## Используя kubectl exec зайдите в любой "под" и попробуйте попинговать "поды" используя FQDN имя соседенего "пода", результаты пингов необходимо приложить к отчету.
