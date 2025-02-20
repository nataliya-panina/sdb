# Домашнее задание к занятию «Защита хоста»

## Задание 1

Установите eCryptfs.  
Добавьте пользователя cryptouser.  
Зашифруйте домашний каталог пользователя с помощью eCryptfs.  
В качестве ответа пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.  

## Решение
Создаю пользователя cryptouser и зашифрованный каталог для него.
```
sudo apt install ecryptfs-utils -y
sudo adduser --encrypt-home cryptouser
```
![image](https://github.com/user-attachments/assets/9fd47957-23d2-415a-a4a5-13133f399b42)
Переключаюсь на только что созданного пользователя и произвожу некоторые действия в его домашнем каталоге:
```
su cryptouser
touch secret.txt grand_secret.txt
```

![image](https://github.com/user-attachments/assets/1b0a33de-8b01-4a5d-9cca-636f6957c3df)  

Затем выхожу из сессии cryptouser и пытаюсь увидеть содержимое его каталога;
Из снимка с экрана видно, что только что созданные файлы были зашифрованы и они не видны другим пользователям.

---
## Задание 2

Установите поддержку LUKS.  
Создайте небольшой раздел, например, 100 Мб.  
Зашифруйте созданный раздел с помощью LUKS.  
В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.  

## Решение
LUKS, pour Linux Unified Key Setup, est le standard GNU/Linux pour le chiffrement des disques.
```
sudo apt install gparted // Установка утилиты для разбиения диска
sudo apt install cryptsetup // Установка 
cryptsetup --version
```

![image](https://github.com/user-attachments/assets/0eaf50f6-c189-4972-a2d5-ee4050b96496)

![image](https://github.com/user-attachments/assets/9e2d290f-5f4b-48e5-a271-b7701baceb39)
```
sudo cryptsetup -y -v --type luks2 luksFormat /dev/sdb1 # Форматирование раздела
sudo cryptsetup luksOpen /dev/sdb1 cryptodisk # Монтирование раздела
ls /dev/mapper/cryptodisk
sudo dd if=/dev/zero of=/dev/mapper/cryptodisk  # Форматирование раздела
sudo mkfs.ext4 /dev/mapper/cryptodisk 
```
![image](https://github.com/user-attachments/assets/0e9f6226-05b1-43f6-9e19-a5ee11672784)

```
mkdir .secret
sudo mount /dev/mapper/cryptodisk .secret/
```
![image](https://github.com/user-attachments/assets/87317c69-3dc8-4d33-8b46-1599206e243b)

Завершение работы:
```
moi@ubu:~$ sudo umount .secret
moi@ubu:~$ sudo cryptsetup luksClose cryptodisk
```
![image](https://github.com/user-attachments/assets/a76246d4-1199-492a-91e1-b83104f432fd)

---
Дополнительные задания (со звёздочкой*)
---
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале

## Задание 3 *

Установите apparmor.  
Повторите эксперимент, указанный в лекции.  
Отключите (удалите) apparmor.  
В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.  
## Решение
