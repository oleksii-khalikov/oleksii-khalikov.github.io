---
title = 'Iptables'
date = 2024-05-26T12:09:00+02:00
draft = false
tags:
  - ubuntu
  - iptables
  - firewall
---

# iptables

Всі команди виконуються під **sudo**

```Bash
sudo su
```

### Перевірка стану

```Bash
iptables -L -nv
```

Також можна вивести результати за таблицями:

* filter (результат буде такий самий, як і без вказання таблиці)

```Bash
iptables -t filter -L -nv
```

* nat

```Bash
iptables -t nat -L -nv
```

Вивести список сервісів, які працюють з мережевими інтерфейсами:

```Bash
ss -ntulp
```

Показати список мережевих інтерфейсів:

```Bash
ip a
```

```
admin@vmi1582341:/home/admin$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:50:56:4d:ec:96 brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    altname ens18
    inet 167.86.321.178/24 brd 167.86.321.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::250:56ff:fe4d:ec96/64 scope link
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:4a:70:83:30 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

Нам треба запам'ятати назву мережі **eth0**, бо вона може бути різною.

### Додавання правил

В першу чергу дозволяємо собі підключення по **ssh**

```Bash
iptables -A INPUT -p tcp --dport=22 -j ACCEPT
```

або з обмеженням по інтерфейсу:

```Bash
iptables -A INPUT -i eth0 -p tcp --dport=22 -j ACCEPT
```

Дозволяємо трафік для внутрішніх інтерфейсів

```Bash
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i docker0 -j ACCEPT
```

Дозволяємо PING-протокол

```Bash
iptables -A INPUT -p icmp -j ACCEPT
```

Додаємо web-серверні порти

```Bash
iptables -A INPUT -p tcp --dport=80 -j ACCEPT
iptables -A INPUT -p tcp --dport=443 -j ACCEPT
```

Дозвіл на отримання відповідей з зовні, які наш сервер очікує:

```Bash
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

#### Закриваємо прийом всіх інших пакетів

```Bash
iptables -P INPUT DROP
```

Якщо після цієї команди страчено доступ - треба перезавантажити сервер, бо правила ще не збережені.

### Збереження правил

```Bash
iptables-save > ./iptables-2024-05-26.rules
```

тут вказати ім'я файлу щоб була можливість потім відновити за допомогою

```Bash
iptables-restore < ./iptables-2024-05-26.rules
```

```Bash
apt install iptables-persistent netfilter-persistent
```

```Bash
netfilter-persistent save
netfilter-persistent start
```

### Заблокувати якийсь IP

```Bash
iptables -I INPUT -s 209.175.153.23 -j ACCEPT
```

## Додаткові матеріали

[Налаштування з нуля](https://youtu.be/Q0EC8kJlB64?si=hdHF3yohljY4zY1i)

[NAT і PORT FORWARDING](https://youtu.be/u_a3ouarrVU?si=pn18mBrBnHpYFuwP)
