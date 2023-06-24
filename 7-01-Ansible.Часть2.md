# Домашнее задание к занятию «Ansible.Часть 2»

### Боробов И.С.

---

### Задание 1

**Выполните действия, приложите файлы с плейбуками и вывод выполнения.**

Напишите три плейбука. При написании рекомендуем использовать текстовый редактор с подсветкой синтаксиса YAML.

Плейбуки должны: 

1. Скачать какой-либо архив, создать папку для распаковки и распаковать скаченный архив. Например, можете использовать [официальный сайт](https://kafka.apache.org/downloads) и зеркало Apache Kafka. При этом можно скачать как исходный код, так и бинарные файлы, запакованные в архив — в нашем задании не принципиально.
2. Установить пакет tuned из стандартного репозитория вашей ОС. Запустить его, как демон — конфигурационный файл systemd появится автоматически при установке. Добавить tuned в автозагрузку.
3. Изменить приветствие системы (motd) при входе на любое другое. Пожалуйста, в этом задании используйте переменную для задания приветствия. Переменную можно задавать любым удобным способом.

### Ответ:
1.
```
---
- name: "Ansible p.2.1.1"
  hosts: servernet
  tasks:

  - name: "Make folfer"
    file:
      path: /root/kafka
      state: directory
  - name: "Unzip"
    ansible.builtin.unarchive:
      src: https://downloads.apache.org/kafka/3.4.0/kafka_2.13-3.4.0.tgz
      dest: /root/kafka
      remote_src: yes

```

2. 

```
---
- name: "Ansible p.2.1.2"
  hosts: servernet
  tasks:

  - name: "Install tuned"
    apt:
      name: tuned
      state: present
  - name: "Autorun tuned"
    ansible.builtin.systemd:
      name: tuned
      state: started
      enabled: true

```

3. 
```
- name: "Ansible p.2.1.3"
  hosts: servernet
  tasks:

  - name: "rewrite"
      ansible.builtin.copy:
        content: "{{ motd }}"
        dest: "/etc/motd"
```
Запускаю так: ansible-playbook ./motd.yaml -v -u borobov --extra-vars "motd=Netology"  

![img-7-01-13](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/49f6decc2afa77069d20809d16bad559a6196cd8/img-7-01/img-7-01-13.png)

Результат:

![img-7-01-14](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/49f6decc2afa77069d20809d16bad559a6196cd8/img-7-01/img-7-01-14.png)

### Задание 2

**Выполните действия, приложите файлы с модифицированным плейбуком и вывод выполнения.** 

Модифицируйте плейбук из пункта 3, задания 1. В качестве приветствия он должен установить IP-адрес и hostname управляемого хоста, пожелание хорошего дня системному администратору. 

### Ответ:

```
- name: "Ansible p.2.2"
  hosts: servernet
  vars:
    mytext: |
      echo Have a good day!
      hostname --all-ip-addresses
      hostname -f
  tasks:
  - name: "rewrite"
    ansible.builtin.copy:
      dest: /etc/profile.d/login.sh
      content: "{{ mytext }}"

```

### Задание 3

**Выполните действия, приложите архив с ролью и вывод выполнения.**

Ознакомьтесь со статьёй [«Ansible - это вам не bash»](https://habr.com/ru/post/494738/), сделайте соответствующие выводы и не используйте модули **shell** или **command** при выполнении задания.

Создайте плейбук, который будет включать в себя одну, созданную вами роль. Роль должна:

1. Установить веб-сервер Apache на управляемые хосты.
2. Сконфигурировать файл index.html c выводом характеристик каждого компьютера как веб-страницу по умолчанию для Apache. Необходимо включить CPU, RAM, величину первого HDD, IP-адрес. Используйте [Ansible facts](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html) и [jinja2-template](https://linuxways.net/centos/how-to-use-the-jinja2-template-in-ansible/)
3. Открыть порт 80, если необходимо, запустить сервер и добавить его в автозагрузку.
4. Сделать проверку доступности веб-сайта (ответ 200, модуль uri).

В качестве решения:
- предоставьте плейбук, использующий роль;
- разместите архив созданной роли у себя на Google диске и приложите ссылку на роль в своём решении;
- предоставьте скриншоты выполнения плейбука;
- предоставьте скриншот браузера, отображающего сконфигурированный index.html в качестве сайта.

### Ответ:

![img-7-01-15](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/49f6decc2afa77069d20809d16bad559a6196cd8/img-7-01/img-7-01-15.png)

**Плайбук:**  
```
- name: "Ansible p.2.3"
  hosts: servernet
  roles:
	- netology
```

**Task из роли:**  
```
---
# tasks file for netology
- name: "Install Apache"
  apt:
	name:
  	apache2
	state: present

- name: "Change Index.html"
  ansible.builtin.template:
	src: "./index.html"
	dest: "/var/www/html"
- name: "Test 200"
  ansible.builtin.uri:
	url: http://localhost
	status_code: 200
- name: "Autostart Apache2"
  ansible.builtin.systemd:
	name: apache2
	state: started
	enabled: true
```

**Содержимое index.html**  
```
<font size="5">

ip_address = {{ansible_all_ipv4_addresses}} <br>
cpu = {{ansible_processor[2]}} <br>
hdd = {{ansible_devices.sda.size}} <br>
ram = {{ansible_memory_mb.real.total}} <br>
</font>
```

**Вывод через web:**  

![img-7-01-16](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/49f6decc2afa77069d20809d16bad559a6196cd8/img-7-01/img-7-01-16.png)
