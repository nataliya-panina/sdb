# Задание 1. Elasticsearch
Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.

Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.

## Решение
compose.yml:
```
services:
  elasticsearch:
    image: elasticsearch:8.12.2
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - cluster.name=nataliyas_cluster
    ports:
      - 9200:9200
```

```
docker compose up -d  
curl -s http://localhost:9200/_cluster/health?pretty  
```

![image](https://github.com/user-attachments/assets/cde67149-3380-4fe4-9e8d-84aba4b699ca)

---
# Задание 2. Kibana
Установите и запустите Kibana.

Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty.

## Решение

В compose.yml дописываю сервис kibana:
```
  kibana:
   image: kibana:8.12.2
   ports:
     - 5601:5601
   depends_on:
    - elasticsearch
   environment:
    - ELASTICSEARCH_HOSTS=http://elasticearch:9200
```
```
docker compose up -d
http://127.0.0.1:5601/app/dev_tools#/console
```

![image](https://github.com/user-attachments/assets/fc767d53-3397-4b3d-ae48-760270443bd6)

---
# Задание 3. Logstash
Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch.

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.

## Решение

---
# Задание 4. Filebeat.
Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat.

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.

## Решение

---
### Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

# Задание 5*. Доставка данных
Настройте поставку лога в Elasticsearch через Logstash и Filebeat любого другого сервиса , но не Nginx. Для этого лог должен писаться на файловую систему, Logstash должен корректно его распарсить и разложить на поля.

Приведите скриншот интерфейса Kibana, на котором будет виден этот лог и напишите лог какого приложения отправляется.

## Решение

---
