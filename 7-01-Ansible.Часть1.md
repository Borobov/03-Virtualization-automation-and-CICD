# Домашнее задание к занятию «Ansible. Часть 1»

### Боробов И.С.

---

### Задание 1

**Ответьте на вопрос в свободной форме.**

Какие преимущества даёт подход IAC?

### Ответ:

1. скорость и уменьшение затрат (настройка инфраструктуры занимает меньше времени);
2. масштабируемость и стандартизация;
3. воспроизводимость (поднимаемая инфраструктура всегда идентична);
4. принципы IaC позволяют не фокусироваться на рутине, а заниматься более важными задачами.

---

### Задание 2 

**Выполните действия и приложите скриншоты действий.**

1. Установите Ansible.
2. Настройте управляемые виртуальные машины, не меньше двух.
3. Создайте файл inventory. Предлагается использовать файл, размещённый в папке с проектом, а не файл inventory по умолчанию.
4. Проверьте доступность хостов с помощью модуля ping.

### Ответ:
1.  
![img-7-01-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-1.png)

2. Сгенерировал ssh ключи и скопировал на управляемы сервера:

ssh-keygen  
ssh-copy-id borobov@192.168.31.183  
ssh-copy-id borobov@192.168.31.184  

3. Создал файл hosts в домашнем каталоге и прописал путь до hosts в ansible.cfg, также предварительно создал конфигурационный файл ansible.cfg

![img-7-01-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-2.png)

![img-7-01-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-3.png)

![img-7-01-4](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-4.png)

4. ansible all -m ping -u borobov

![img-7-01-5](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-5.png)

![img-7-01-6](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-6.png)

---

### Задание 3 

**Ответьте на вопрос в свободной форме.**

Какая разница между параметрами forks и serial? 

### Ответ:

forks - настраивает количество параллельных процессов, которые может запускть Ansible, и, следовательно, максимальное количество хостов, которые можно настроить параллельно. Значение --fork по умолчанию равно 5.  

serial - можно задать меньшее значение, чем параметр --forks. Это количество хостов, на которых будут запущено параллельно задание или сценарий, для которых установлен параметр serial.  

Комбинация полезна, так как она позволяет вам настроить свой playbook для работы с лучшим количеством хостов для производительности, но затем ограничить определенные задачи до этого значения (например, чтобы гарантировать, что перезапуск службы будет поэтапным, а не применяется ко всем хостов одновременно, что гарантирует отсутствие простоев этой службы с точки зрения конечных пользователей).  

---

### Задание 4 

В этом задании вы будете работать с Ad-hoc коммандами.

**Выполните действия и приложите скриншоты запуска команд.**

1. Установите на управляемых хостах любой пакет, которого нет.
2. Проверьте статус любого, присутствующего на управляемой машине, сервиса. 
3. Создайте файл с содержимым «I like Linux» по пути /tmp/netology.txt.
 
### Ответ:

1. ansible all -m apt -a "name=mc state=present" -u borobov  
![img-7-01-7](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-7.png)

![img-7-01-8](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-8.png)

2. ansible test -m service -a "name=cron" -u borobov  
![img-7-01-9](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-9.png)

3. Создаю файл: ansible all -m ansible.builtin.file -a "path=/tmp/netology.txt state=touch" -u borobov  
![img-7-01-10](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-10.png)

Записываю содержимое в файл: ansible all -m blockinfile -a "path=/tmp/netology.txt block='I like Linux'" -u borobov  
![img-7-01-11](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-11.png)

Результат на одной из управляемых машин  
![img-7-01-12](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/3e4322cea83cf793e805c02e12b9e5d6c0228031/img-7-01/img-7-01-12.png)
