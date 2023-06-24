# Домашнее задание к занятию «Kubernetes. Часть 1»
### Боробов И С

---

### Задание 1

**Выполните действия:**

1. Запустите Kubernetes локально, используя k3s или minikube на свой выбор.
1. Добейтесь стабильной работы всех системных контейнеров.
2. В качестве ответа пришлите скриншот результата выполнения команды kubectl get po -n kube-system.

### Ответ:

![img-6-05-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3bdf79bb964b56157b20a366965c784657da9f59/img-6-05/img-6-05-1.png)

------
### Задание 2


Есть файл с деплоем:

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis
        env:
         - name: REDIS_PASSWORD
           value: password123
        ports:
        - containerPort: 6379
```

------
**Выполните действия:**

1. Измените файл с учётом условий:

 * redis должен запускаться без пароля;
 * создайте Service, который будет направлять трафик на этот Deployment;
 * версия образа redis должна быть зафиксирована на 6.0.13.

2. Запустите Deployment в своём кластере и добейтесь его стабильной работы.
3. В качестве решения пришлите получившийся файл.

### Ответ:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
	matchLabels:
  	app: redis
  replicas: 1
  template:
	metadata:
  	labels:
    	app: redis
	spec:
  	containers:
  	- name: master
    	image: bitnami/redis:6.0.13 # добавил
    	env:
      	- name: ALLOW_EMPTY_PASSWORD # добавил
        	value: "yes" # добавил
    	ports:
      	- containerPort: 6379
apiVersion: v1 # добавил
kind: Service # добавил
metadata: # добавил
  name: redis # добавил
spec: # добавил
  selector: # добавил
	app: redis # добавил
  ports: # добавил 
	- protocol: TCP # добавил
  	port: 6379 # добавил

```
![img-6-05-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3bdf79bb964b56157b20a366965c784657da9f59/img-6-05/img-6-05-2.png)

![img-6-05-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3bdf79bb964b56157b20a366965c784657da9f59/img-6-05/img-6-05-3.png)

------
### Задание 3

**Выполните действия:**

1. Напишите команды kubectl для контейнера из предыдущего задания:

 - выполнения команды ps aux внутри контейнера;
 - просмотра логов контейнера за последние 5 минут;
 - удаления контейнера;
 - проброса порта локальной машины в контейнер для отладки.

2. В качестве решения пришлите получившиеся команды.

### Ответ:

**Подключился к контейнеру**  
kubectl exec -it redis-9f87ddf8b-dxzz7 bash  
ps aux  
![img-6-05-4](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3bdf79bb964b56157b20a366965c784657da9f59/img-6-05/img-6-05-4.png)

**Лог за 5 минут**  
kubectl logs --since=5m redis-9f87ddf8b-dxzz7  

**Удаление контейнера**  
kubectl delete po -f redis.yaml  

**Проброс порта локальной машины в контейнер**  
kubectl port-forward service/redis 6379  

![img-6-05-5](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3bdf79bb964b56157b20a366965c784657da9f59/img-6-05/img-6-05-5.png)

------
