### University: [ITMO University](https://itmo.ru/ru/)
### Faculty: [FICT](https://fict.itmo.ru)
### Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
### Year: 2022/2023
### Group: K4111c
### Author: Sokolova Anna Aleksandrovna
### Lab: Lab1
### Date of create: 27.11.2022
### Date of finished: 

# Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."
## Цель работы
Ознакомиться с инструментами Minikube и Docker, развернуть "под"
## Ход работы
## Установка Docker
Установка Docker Engine:

Обновление существующих списков пакетов:

```
sudo apt update
```

Установка обязательных пакетов, которые позволяют apt использовать пакеты по HTTPS:

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Добавление ключа GPG официального репозитория Docker в систему:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Добавление репозитория Docker:

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Обновление списка пакетов:

```
sudo apt update
```

Указание установки из репозитория Docker:

```
apt-cache policy docker-ce
```

Установка Docker:

```
sudo apt install docker-ce
```

Проверка работоспособности Docker:

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/a61d9a6e8a096cc3a47916de72732467d62fb78a/lab1/images/status_docker.jpg)

Установим Docker Compose:
Запуск команды для установки последней версии docker-compose:
```
curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
Проверка установки docker-compose:

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/0708cf9e31d9066411591e32a940dc3f7475f15b/lab1/images/docker-compose.jpg)

## Установка  minikube 
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## Запуск minikube cluster
```
minikube start
```

## Взаимодействие с k8s
```
minikube kubectl
```

## Написание манифеста для развертывания "пода" HashiCorp Vault, и проброс внутрь порта 8200
```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: vault
  namespace: lesson1
  labels: 
    app: vault
spec:
  containers:
  - name: vault
    image: vault:1.12.1
    ports:
    - containerPort: 8200
```

Создание пода:
```
kubectl apply -f vaultpod.yml
```
Создание сервиса для доступа к контейнеру:
```
kubectl expose pod vault --port=8200 --type=NodePort
```
kubectl port-forward позволяет использовать имя ресурса для выбора соответствующего модуля для переадресации порта:
```
minikube kubectl -- port-forward pod/vault 8200:8200
```

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/0708cf9e31d9066411591e32a940dc3f7475f15b/lab1/images/port-forward.jpg)

Вывод логов vault:
```
kubectl logs vault
```

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/0708cf9e31d9066411591e32a940dc3f7475f15b/lab1/images/token.jpg)

Вход в vault, используя токен:

![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/f447103bfabb0f9159708f10455562b8c2f793fe/lab1/images/Vault.png)

## Создание схемы организации контейнеров и сервисов
![Image text](https://github.com/AnyaSok/2022_2023-introduction_to_distributed_technologies-k4111c-sokolova_a_a/blob/4c16e420b4afd1922d67b9cc0bc271dc6fa946d7/lab1/images/Diagram.png)



