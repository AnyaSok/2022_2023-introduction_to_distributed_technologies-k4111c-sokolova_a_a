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

```sudo apt install apt-transport-https ca-certificates curl software-properties-common```

Добавление ключа GPG официального репозитория Docker в систему:

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

Добавление репозитория Docker:

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

Обновление списка пакетов:

`sudo apt update`

Указание установки из репозитория Docker:

`apt-cache policy docker-ce`

Установка Docker:

`sudo apt install docker-ce`

Проверка работоспособности Docker:

Установим Docker Compose:
Запуск команды для установки последней версии docker-compose:
```
curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
Проверка установки docker-compose:

## Установка  minikube 
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## Запуск minikube cluster
`minikube start`

## Взаимодействие с k8s
`minikube kubectl`

## Написание манифеста для развертывания "пода" HashiCorp Vault, и проброс внутрь порта 8200

Создание пода:
`kubectl apply -f vaultpod.yml`
Создание сервиса для доступа к контейнеру:
`kubectl expose pod vault --port=8200 --type=NodePort`
kubectl port-forward позволяет использовать имя ресурса для выбора соответствующего модуля для переадресации порта:
`minikube kubectl -- port-forward pod/vault 8200:8200`

Вывод логов vault:
`kubectl logs vault`

Вход в vault, используя токен:

## Создание схемы организации контейнеров и сервисов




