# Домашнее задание к занятию «Типы виртуализации: KVM, QEMU»

### Боробов И.С.

---

### Задание 1

**Ответьте на вопрос в свободной форме.**

Какие виртуализации существуют? Приведите примеры продуктов разных типов виртуализации.

### Ответ:

Аппаратная - обеспечивает производительность, сравнимую с производительностью невиртуализованной машины, что дает виртуализации возможность практического использования и влечет её широкое распространение. Наиболее распространены технологии виртуализации Intel-VT и AMD-V.  

Программная - всё железо для виртуальной машины эмулируется программо. DosBOX  

Контейнерная - метод виртуализации, при котором ядро операционной системы поддерживает несколько изолированных экземпляров пространства пользователя вместо одного. Эти экземпляры с точки зрения выполняемых в них процессов идентичны отдельному экземпляру операционной системы. Docker, LXC.  

Хостинговая - виртуализация на хостинге провайдера. AWS, GCP, Яндекс.Облако.  

---

### Задание 2 

Выполните действия и приложите скриншоты по каждому этапу:

1. Установите QEMU в зависимости от системы (в лекции рассматривались примеры).
2. Создайте виртуальную машину.
3. Установите виртуальную машину.
Можете использовать пример [по ссылке](https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/x86/alpine-standard-3.13.5-x86.iso).

Пример взят [с сайта](https://alpinelinux.org). 

### Ответ: 

1.	sudo apt install qemu-kvm qemu qemu-system
2.	qemu-img create -f qcow2 hdd.qcow 10G
3.	qemu-system-x86_64 -hda hdd.qcow -boot d -cdrom ~/alpine-standard-3.13.5-x86.iso -m 640
4.
![img-6-02-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-1.png)

---

### Задание 3 

Выполните действия и приложите скриншоты по каждому этапу:

1. Установите KVM и библиотеку libvirt. Можете использовать GUI-версию из лекции. 
2. Создайте виртуальную машину. 
3. Установите виртуальную машину. 
Можете использовать пример [по ссылке](https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/x86/alpine-standard-3.13.5-x86.iso). 

Пример взят [с сайта](https://alpinelinux.org). 

### Ответ:

1.	Проверим поддерживается libvirt  
egrep --color=auto 'vmx|svm|0xc0f' /proc/cpuinfo  
2.	Проверим загружен ли KVM: lsmod | grep kvm  
![img-6-02-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-2.png)
3.	Установим необходимые пакеты: sudo apt install “Virtual*”
4.	Установим virt manager: sudo apt install virt-manager
5.	![img-6-02-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-3.png)
6. ![img-6-02-4](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-4.png)


 ---

### Задание 4

Выполните действия и приложите скриншоты по каждому этапу:

1. Создайте проект в GNS3, предварительно установив [GNS3](https://github.com/GNS3/gns3-gui/releases).
2. Создайте топологию, как на скрине ниже.
3. Для реализации используйте машину на базе QEMU. Можно дублировать, сделанную ранее. 

![image](https://user-images.githubusercontent.com/73060384/118615008-f95e9680-b7c8-11eb-9610-fc1e73d8bd70.png)

### Ответ:

Установил GNS  
sudo apt update  
sudo apt install -y python3-pip python3-pyqt5 python3-pyqt5.qtsvg \  
python3-pyqt5.qtwebsockets \  
qemu qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst \  
wireshark xtightvncviewer apt-transport-https \  
ca-certificates curl gnupg2 software-properties-common  

sudo pip3 install gns3-server  
sudo pip3 install gns3-gui  

sudo apt install dynamips (без Dynamips не работало сетевое оборудование в GNS)  

Запустил GNS: sudo gns3  
Создал виртуальную машину (Edit - Preferences - QEMU - Qemu VMs - New VM) и подключил образ iso операционной системы, определил объем диска, объем ОЗЦ, загрузил систему и подключается к консоли.  
![img-6-02-5](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-5.png)
![img-6-02-6](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/d416672c928db3314d98afd204edf44e871d4b70/img-6-02/img-6-02-6.png)

