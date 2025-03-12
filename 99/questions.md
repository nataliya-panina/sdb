Вопрос по logstash:
compose.yml
```yml
services:
  elasticsearch:
    image: elasticsearch:8.12.2
    environment:
    - discovery.type=single-node
    - xpack.security.enabled=false
    ports:
    - 9200:9200

  kibana:
    image: kibana:8.12.2
    ports:
    - "5601:5601"
    depends_on:
    - elasticsearch
    environment:
    - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
  logstash:
    image: logstash:8.12.2
    environment:
      ES_HOST: "elasticsearch:9200"
      
    ports:
    - "5044:5044/udp"
    volumes:
    - ./configs/logstash/config.yml:/usr/share/logstash/config/logstash.yml
    - ./configs/logstash/pipelines.yml:/usr/share/logstash/config/pipelines.yml
    - ./configs/logstash/pipelines:/usr/share/logstash/config/logstash
    depends_on:
    - elasticsearch
```

pypelines.yml
```
- pipeline.id: service_stamped_json_logs
  pipeline.workers: 1
  pipeline.batch.size: 1
  path.config: "/usr/share/logstash/config/pipelines/udp_service_logs_es.conf"
```
