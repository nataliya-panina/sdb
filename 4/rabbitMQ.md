# Домашнее задание к занятию «Очереди RabbitMQ» - Панина Наталия
------
# Задание 1. Установка RabbitMQ

Используя Vagrant или VirtualBox, создайте виртуальную машину и установите RabbitMQ. Добавьте management plug-in и зайдите в веб-интерфейс.

Итогом выполнения домашнего задания будет приложенный скриншот веб-интерфейса RabbitMQ.

## Решение

compose.yml:  

```yaml
services:
  rabbitmq1:
    image: rabbitmq:3.10.7-management
    hostname: rabbitmq1
    restart: always
    environment:
      - RABBITMQ_DEFAULT_USER=${RABBITMQ_DEFAULT_USER} # переменные окружения описаны в файле .env
      - RABBITMQ_DEFAULT_PASS=${RABBITMQ_DEFAULT_PASS}
      - RABBITMQ_CONFIG_FILE=/config/rabbitmq
      - RABBITMQ_NODE_PORT=5672
    volumes:
      - ./config:/config
    ports:
      - 15672:15672 # порт для обращения через вебинтерфейс
      - 5672:5672 # порт для общения с приложениями
```
```
docker compose up -d
```
![image](https://github.com/user-attachments/assets/4cc14404-aaef-4246-8659-ae9f4ca0542b)

---
# Задание 2. Отправка и получение сообщений
Используя приложенные скрипты, проведите тестовую отправку и получение сообщения. Для отправки сообщений необходимо запустить скрипт producer.py.

Для работы скриптов вам необходимо установить Python версии 3 и библиотеку Pika. Также в скриптах нужно указать IP-адрес машины, на которой запущен RabbitMQ, заменив localhost на нужный IP.
```bash
$ pip install pika
```


Зайдите в веб-интерфейс, найдите очередь под названием hello и сделайте скриншот. После чего запустите второй скрипт consumer.py и сделайте скриншот результата выполнения скрипта

В качестве решения домашнего задания приложите оба скриншота, сделанных на этапе выполнения.

Для закрепления материала можете попробовать модифицировать скрипты, чтобы поменять название очереди и отправляемое сообщение.

## Решение
[producer.py](https://github.com/nataliya-panina/sdb/blob/main/4/producer.py), [consumer.py](https://github.com/nataliya-panina/sdb/blob/main/4/consumer.py)
```bash
pip install pika
# Создание и активация виртуального окружения python:
python3 -m venv venv
source venv/bin/activate
python producer.py
```
![rabbit_web_queue](https://github.com/user-attachments/assets/f2908b0f-71df-4182-8f9c-19ad28475e45)

```
python producer.py
python consumer.py
```

![terminal_producer_consumer](https://github.com/user-attachments/assets/6ec4dc57-bef5-4c46-b348-53ad1e2464a9)



---
# Задание 3. Подготовка HA кластера
Используя Vagrant или VirtualBox, создайте вторую виртуальную машину и установите RabbitMQ. Добавьте в файл hosts название и IP-адрес каждой машины, чтобы машины могли видеть друг друга по имени.

Пример содержимого hosts файла:

$ cat /etc/hosts
192.168.0.10 rmq01
192.168.0.11 rmq02
После этого ваши машины могут пинговаться по имени.

Затем объедините две машины в кластер и создайте политику ha-all на все очереди.

В качестве решения домашнего задания приложите скриншоты из веб-интерфейса с информацией о доступных нодах в кластере и включённой политикой.

Также приложите вывод команды с двух нод:

$ rabbitmqctl cluster_status
Для закрепления материала снова запустите скрипт producer.py и приложите скриншот выполнения команды на каждой из нод:

$ rabbitmqadmin get queue='hello'
После чего попробуйте отключить одну из нод, желательно ту, к которой подключались из скрипта, затем поправьте параметры подключения в скрипте consumer.py на вторую ноду и запустите его.

Приложите скриншот результата работы второго скрипта.

## Решение


![image](https://github.com/user-attachments/assets/9d9d30bf-20b2-4e15-b2ba-6661b9879c09)
![image](https://github.com/user-attachments/assets/ea54f5c2-2f11-42fb-88c1-5b586d7fde6e)
![image](https://github.com/user-attachments/assets/0fe8a281-4f10-4554-8899-485ad47d4522)
![image](https://github.com/user-attachments/assets/ce83cc5d-5ed7-498c-b2d7-a1e1820b1519)
![image](https://github.com/user-attachments/assets/3bcbc0fb-72a7-46ef-ac98-d834c78b3a35)


---
### Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

# * Задание 4. Ansible playbook
Напишите плейбук, который будет производить установку RabbitMQ на любое количество нод и объединять их в кластер. При этом будет автоматически создавать политику ha-all.

Готовый плейбук разместите в своём репозитории.

## Решение

---
