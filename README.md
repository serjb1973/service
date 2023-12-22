## Домашнее задание: Services
### Цель:
#### Получить навыки работы с timer
#### Научиться использовать шаблоны в service

### Для выполнения работы используются следующие инструменты:
- VirtualBox - среда виртуализации, позволяет создавать и выполнять виртуальные машины;
- Vagrant - ПО для создания и конфигурирования виртуальной среды. В данном случае в качестве среды виртуализации используется VirtualBox;
- Ansible - Система управления конфигурациями, написанная на языке программирования Python;
- Git - система контроля версий

### Аккаунты:
- GitHub - https://github.com/

### Создание образа
#### service
```
vagrant up
```
### Подготовка ssh 
```
ssh-keygen -R "[127.0.0.1]:2222" -f ~/.ssh/known_hosts
ssh -p 2222 vagrant@127.0.0.1 -i ~/.ssh/known_hosts
ansible all -m ping -i hosts.ini
```
### Настройка watchlog.service, watchlog.timer и дополнительных конфигов
```
ansible-playbook -i hosts.ini service_1.yml
```
### Проверка
```
ansible service -m shell -a 'sleep 30 ; tail -n 100 /var/log/messages|grep -E "Master"|grep -v ansible' -i hosts.ini -b
```
### Установка spawn-fcgi и необходимых для него пакетов:
```
ansible-playbook -i hosts.ini service_2.yml
```
### Запуск spawn-fcgi
### Запуск двух экземпляров httpd
```
ansible-playbook -i hosts.ini service_3.yml
```
### Проверка
```
ansible service -m shell -a 'systemctl status spawn-fcgi' -i hosts.ini -b
ansible service -m shell -a 'ss -tnulp | grep httpd' -i hosts.ini -b
```
