# Домашнее задание к занятию «Kubernetes. Часть 2»

### Боробов И.С.

---

### Задание 1

**Выполните действия:**

1. Создайте свой кластер с помощью kubeadm.
1. Установите любой понравившийся CNI плагин.
1. Добейтесь стабильной работы кластера.

В качестве ответа пришлите скриншот результата выполнения команды `kubectl get po -n kube-system`.

### Ответ:

![img-6-06-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d162ef7c5a459e3d24485ea317277c2e5175b45c/img-6-06/img-6-06-1.png)

---

### Задание 2

Есть файл с деплоем:

```yaml
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
**Выполните действия:**

1. Создайте Helm Charts.
1. Добавьте в него сервис.
1. Вынесите все нужные, на ваш взгляд, параметры в `values.yaml`.
1. Запустите чарт в своём кластере и добейтесь его стабильной работы.

В качестве ответа пришлите вывод команды `helm get manifest <имя_релиза>`.

###  Ответ:

![img-6-06-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d162ef7c5a459e3d24485ea317277c2e5175b45c/img-6-06/img-6-06-2.png)


![img-6-06-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d162ef7c5a459e3d24485ea317277c2e5175b45c/img-6-06/img-6-06-3.png)

```
root@netology-deb10:~# helm get manifest my-project-1678027431
---
# Source: my-project/templates/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-project-1678027431
  labels:
	helm.sh/chart: my-project-0.1.0
	app.kubernetes.io/name: my-project
	app.kubernetes.io/instance: my-project-1678027431
	app.kubernetes.io/version: "1.16.0"
	app.kubernetes.io/managed-by: Helm
---
# Source: my-project/templates/service.yaml
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
    	image: bitnami/redis:6.0.13
    	env:
      	- name: ALLOW_EMPTY_PASSWORD
        	value: "yes"
    	ports:
     	- containerPort: 6379
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  selector:
	app: redis
  ports:
	- protocol: TCP
  	port: 6379
---
# Source: my-project/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
	matchLabels:
  	app: redis
  replicas: 4
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

---
