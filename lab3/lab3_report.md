### University: [ITMO University](https://itmo.ru/ru/)
### Faculty: [FICT](https://fict.itmo.ru)
### Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
### Year: 2022/2023
### Group: K4111c
### Author: Sokolova Anna Aleksandrovna
### Lab: Lab3
### Date of create: 27.11.2022
### Date of finished: 


# Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."
## Цель работы
Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.
## Ход работы
## Создание configMap с переменными: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: frontend-configmap
data:
  react_app_username: "Anna"
  react_app_company_name: "ITMO_University"
```
## Применение манифеста
```
kubectl apply -f configMap.yml
```
## Создание replicaSet
Написание манифеста для создания replicaSet с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передача переменных REACT_APP_USERNAME, REACT_APP_COMPANY_NAME, используя configMap.
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs
  labels:
    app: lab3
spec:
  replicas: 2
  selector:
    matchLabels:
      app: lab3
  template:
    metadata:
      labels:
        app: lab3
    spec:
      containers:
        - name: rs-pod
          image: ifilyaninitmo/itdt-contained-frontend:master
          env:
            - name: REACT_APP_USERNAME
              valueFrom:
                configMapKeyRef:
                  name: rs-configmap
                  key: react_app_username
            - name: REACT_APP_COMPANY_NAME
              valueFrom:
                configMapKeyRef:
                  name: rs-configmap
                  key: react_app_company_name
          ports:
            - containerPort: 3000
```
## Применение манифеста
```
kubectl apply -f replicaSet.yml
```
## Создание сервиса, через который будет доступ на pods
```
 apiVersion: v1
kind: Service
metadata:
  name: rs-service
spec:
  selector:
    app: lab3
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 3000
  type: NodePort
```
## Применение манифеста
```
kubectl apply -f service.yml
```
## Включение minikube addons enable ingress
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/eb4e9c9ed8ff52baa5702b817b7daa84dc2a7a77/lab3/images/minikubeaddons.png)
## Генерация TLS сертификата и импорт сертификата в minikube
Генерация TLS сертификата
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/1139931eff191e8713ee78bbec050afeedbb0385/lab3/images/tls.png)
Создание TLS secret

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/6ca427fc07d9152ac530637f159f09b775762a5a/lab3/images/secret.png)
## Создать ingress 
Создание ingress в minikube, где указан ранее импортированный сертификат, FQDN, по которому будет выполнен вход и имя сервиса, который был создан ранее.
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress
spec:
  tls:
  - hosts:
      - front.anna.com
    secretName: secret-tls
  rules:
  - host: front.anna.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: rs-service
            port:
              number: 8080
```
## Применение манифеста
```
kubectl apply -f ingress.yml
```
## В hosts запись FQDN и IP адрес ingress
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/bea02c6ab63a9e0a81662339f38dff8b07e73ace/lab3/images/hosts.png)
## Вход в веб-приложение по указанному ранее FQDN используя HTTPS и проверка наличия сертификата
ход в веб-приложение по указанному ранее FQDN используя HTTPS
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/b76d7de63cd07990471c736f196daf59b85f6a0c/lab3/images/front.anna.com.png)

Проверка наличия сертификата

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/aa4308ae244399f10fa82f59e13d6bf53167939c/lab3/images/certificate.png)
## Создание схемы организации контейнеров и сервисов
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/411fe97bcb4e7ce93447d23d228ac4e5e1092a95/lab3/images/Diagram.png)
