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

Проверка успешного создания nodes.

![Image text]()

