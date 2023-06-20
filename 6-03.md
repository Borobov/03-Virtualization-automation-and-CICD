# Домашнее задание к занятию «Docker. Часть 1»
### Боробов И.С.
---

### Задание 1

**Ответьте на вопрос в свободной форме.** 

Чем контейнеризация отличается от виртуализации?

### Ответ:

Основное различие контейнеров и виртуальных машин заключается в том, что виртуальные машины виртуализируют весь компьютер вплоть до аппаратных уровней, а контейнеры — только программные уровни выше уровня операционной системы. Контейнер более легковесен, чем классическая виртуальная машина, так как виртуальной машине требуется изолированная среда (полноценная установка ОС, что требует больше ресурсов ЦП, ОЗУ и т.д.).  

**Контейнер** - это набор процессов, изолированный от остальной операционной системы и запускаемый с отдельного образа, который содержит все файлы, необходимые для их работы. Образ содержит все зависимости приложения и поэтому может легко переноситься из среды разработки в среду тестирования, а затем в промышленную среду.  

---

### Задание 2 

**Выполните действия:**

1. Установите Docker.
2. Приложите скриншот.

### Ответ:
Установку Docker проводил по инструкции с официального сайта: https://docs.docker.com/engine/install/debian/#set-up-the-repository  

Ниже приведу часть листинга процесса установки Docker:  

![img-6-03-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-1.png)  

---

### Задание 3

**Выполните действия:**

1. Запустите образ hello-world.
2. Приложите скриншот.

### Ответ:
![img-6-03-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-2.png)  

---

### Задание 4 

**Выполните действия:**

1. Удалите образ hello-world.
2. Приложите скриншот.

### Ответ:

1.	посмотрим все контейнеры
![img-6-03-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-3.png)

2.	удалим все контейнеры использующие образ hello-world
![img-6-03-4](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-4.png)
3.	удалим образ hello-world
![img-6-03-5](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-5.png)  

---

## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 5*

1. Найдите в Docker Hub образ apache и установите его.
2. Приложите:
 * скриншоты сетевых настроек вашей виртуальной машины;
 * скриншоты работающих контейнеров;
 * скриншот браузера, где вы открыли дефолтную страницу вашего apache внутри контейнера.

### Ответ:

![img-6-03-6](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-6.png)  
![img-6-03-7](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-7.png)  
![img-6-03-8](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-8.png)  

---

### Задание 6*

1. Создайте свой Docker образ с Apache2 и подмените стандартную страницу index.html на страницу, содержащую ваши ФИО.
2. Приложите:
 * скриншот содержимого Dockerfile;
 * скриншот браузера, где apache2 из вашего контейнера выводит ваши ФИО.

### Ответ:
![img-6-03-9](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-9.png)  
![img-6-03-10](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/c144e0b041e7a723ef433ca2f9ae5deca6fac72c/img-6-03/img-6-03-10.png)  
