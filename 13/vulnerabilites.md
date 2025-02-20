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
nmap --help:  
```
moi@ubu:~$ nmap --help
Nmap 7.95 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN, TCP ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan

sudo snap connect nmap:network-control
```
SYN:  
```
sudo nmap -sS 192.168.115.132

```
![image](https://github.com/user-attachments/assets/8a16cab3-7d68-4601-bb2f-37742b65abed)

В этом режиме на удаленный хост отправляется сигнал на открытие соединения SYN. Возвращаются флаги RST, ACK

FIN:
```
nmap -sF 192.168.115.132
```
![image](https://github.com/user-attachments/assets/fd5e43b9-617c-4d29-93ea-13693fd57096)  

В этом режиме на хост посылается пакет с флагом FIN - сигнал на закрытие соединения.


Xmas:  
```
nmap -sX 192.168.115.132
```
![image](https://github.com/user-attachments/assets/befa6762-cae2-4e66-822d-d99b4aaaf35e)

В этом режиме отправляются три флага: FIN, PSH и URG:

UDP:  
```
nmap -sU 192.168.115.132
```
![image](https://github.com/user-attachments/assets/cb06bd28-ae09-4766-8d95-adc58b08e421)  

В отличие от tcp, протокол udp не требует синхронизации, поэтому при установлении соединения не отправляются параметры tcp-окна.
