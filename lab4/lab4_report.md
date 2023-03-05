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

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/getnodesall.png)

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/describenodes.png)

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/getnodes.png)

## Проверка работы Calico с помощью функции IPAM Plugin.
Для проверки режима IPAM необходимо для запущеных ранее нод указать label по признаку стойки или географического расположения

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/label.png)

## Разработка манифеста для Calico, который на основе ранее указанных меток назначает бы IP адреса "подам", исходя из пулов IP адресов которые указаны в манифесте.

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/ippool.png)

```
calicoctl create --allow-version-mismatch -f pool1.yml
calicoctl create --allow-version-mismatch -f pool2.yml
```
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/getippool.png)

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

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/getpods.png)

## Создать сервис через который у вас будет доступ на эти "поды"

```
kubectl expose deployment lab4 --port=3000 --type=LoadBalancer
```

## Запустить в minikube режим проброса портов и подключитесь к вашим контейнерам через веб браузер

```
minikube kubectl -- port-forward deployment/lab4 3000:3000
```

## Проверка на странице в веб-браузере переменных REACT_APP_USERNAME, REACT_APP_COMPANY_NAME и Container name

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/web.png)

## Используя kubectl exec зайдите в любой "под" и попробуйте попинговать "поды" используя FQDN имя соседенего "пода", результаты пингов необходимо приложить к отчету.

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/ping1.png)

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/558a9f8395b420852c193fa0b8974c6c17157df4/lab4/images/ping2.png)

## Создание схемы организации контейнеров и сервисов
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/2483eec4fed22501b00765734ccfbfcbfc743d8d/lab4/images/diagram.png)
