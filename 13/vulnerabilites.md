# Домашнее задание к занятию «Уязвимости и атаки на информационные системы»

## Задание 1
Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/. 

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя nmap.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:  

Какие сетевые службы в ней разрешены?  
Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)  
Приведите ответ в свободной форме.  

---
## Решение
![image](https://github.com/user-attachments/assets/406428df-8a8f-4ca8-82e4-8bedd0b21501)

На снимке с экрана видно, что в исследуемой ОС 23 открытых порта, таких, как 21/ftp, 22/ssh, 23/telnet, 53/DNS, 80/http, 3306/Mysql, и т.д

Уязвимости (https://www.exploit-db.com/):  

- ![PostgreSQL 8.3.6 - Conversion Encoding Remote Denial of Service](https://www.exploit-db.com/exploits/32849)  - Exploiting this issue may allow attackers to terminate connections to the PostgreSQL server, denying service to legitimate users.  
- ![UnrealIRCd 3.x - Remote Denial of Service](https://www.exploit-db.com/exploits/27407) -  A remote attacker may exploit this issue to deny service for legitimate users.  
- ![UnrealIRCd 3.2.8.1 - Remote Downloader/Execute](https://www.exploit-db.com/exploits/13853) - Remote Downloader/Execute Trojan
  

---
## Задание 2
Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
Как отвечает сервер?
Приведите ответ в свободной форме.
## Решение

