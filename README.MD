# Домашнее задание к занятию "Уязвимости и атаки на информационные системы" - Колесин Владимир

### Задание 1.

*Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.*  
*Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.*  
*Просканируйте эту виртуальную машину, используя nmap.*  
*Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.*  
*Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.*  
*Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.*  
*Ответьте на следующие вопросы:*  
*Какие сетевые службы в ней разрешены?*  
*Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)*  
*Приведите ответ в свободной форме.* 

На тестовой VMке разрешены следующие службы:  
```
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
```

На тестовой машине работает MySQL сервер версии 5.0.51a, есть вероятность что данный сервер обладает уязвимостью  
MySQL 4.x/5.0 (Linux) - User-Defined Function (UDF) Dynamic Library (2)  
https://www.exploit-db.com/exploits/1518  

Так же обнаружен сервер PostgreSQL DB 8.3.0, который может обладать уязвимостью  
PostgreSQL 8.3.6 - Conversion Encoding Remote Denial of Service  
https://www.exploit-db.com/exploits/32849  

Помимо этого на виртуалке замечена служба Samba 3.0.20, с уязвимостью  
Samba < 3.0.20 - Remote Heap Overflow  
https://www.exploit-db.com/exploits/7701

### Задание 2. 

*Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.*  
*Запишите сеансы сканирования в Wireshark.*  
*Ответьте на следующие вопросы:*  
*Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?*  
*Как отвечает сервер?*  
*Приведите ответ в свободной форме.*  

Сеансы сканирования wireshark:  
[SYN](https://github.com/Villigun/netology-sdb-13-01-hw/blob/master/nmap-SYN.pcapng)  
[FIN](https://github.com/Villigun/netology-sdb-13-01-hw/blob/master/nmap-FIN.pcapng)  
[Xmas](https://github.com/Villigun/netology-sdb-13-01-hw/blob/master/nmap-Xmas.pcapng)  
[UDP](https://github.com/Villigun/netology-sdb-13-01-hw/blob/master/nmap-UDP.pcapng)  

Исходя из анализа траффика можно сделать вывод что  
  при запросе SYN для открытых портов следует ответ SYN-ACK, для закрытых RST;  
  при запросе FIN для открытых портов ответ отсутствует, для закрытых портов следует ответ RST;  
  при запросе Xmas для открытых портов ответ отсутствует, для закрытых портов следует ответ RST;  
  при запросе UDP для открытых портов ответ отсутствует, для закрытых портов следует ответ ICMP - порт закрыт;

Опираясь на различия ответов на запросы, при комплексном сканировании можно однозначно определить открытые порты на тестовой виртуалке.  
