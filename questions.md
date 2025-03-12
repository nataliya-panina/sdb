# Вопрос по logstash:  
Я пытаюсь повторить то, что делал преподаватель на лекции, у меня при запуске стека ЕЛК logstash через минуту падает, вот его логи:  
```
 *  Executing task: docker logs --tail 1000 -f 7746f7f2dc9db8a589812eeee1dc319ed7a2073d7292fca425e08cbe31d24576 

Using bundled JDK: /usr/share/logstash/jdk
/usr/share/logstash/vendor/bundle/jruby/3.1.0/gems/concurrent-ruby-1.1.9/lib/concurrent-ruby/concurrent/executor/java_thread_pool_executor.rb:13: warning: method redefined; discarding old to_int
/usr/share/logstash/vendor/bundle/jruby/3.1.0/gems/concurrent-ruby-1.1.9/lib/concurrent-ruby/concurrent/executor/java_thread_pool_executor.rb:13: warning: method redefined; discarding old to_f
Sending Logstash logs to /usr/share/logstash/logs which is now configured via log4j2.properties
[2025-03-12T13:26:34,834][INFO ][logstash.runner          ] Log4j configuration path used is: /usr/share/logstash/config/log4j2.properties
[2025-03-12T13:26:34,849][INFO ][logstash.runner          ] Starting Logstash {"logstash.version"=>"8.12.2", "jruby.version"=>"jruby 9.4.5.0 (3.1.4) 2023-11-02 1abae2700f OpenJDK 64-Bit Server VM 17.0.10+7 on 17.0.10+7 +indy +jit [x86_64-linux]"}
[2025-03-12T13:26:34,854][INFO ][logstash.runner          ] JVM bootstrap flags: [-XX:+HeapDumpOnOutOfMemoryError, -Dlogstash.jackson.stream-read-constraints.max-number-length=10000, --add-opens=java.base/java.nio.channels=ALL-UNNAMED, --add-exports=jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED, -Djruby.regexp.interruptible=true, --add-opens=java.base/java.security=ALL-UNNAMED, --add-exports=jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED, --add-exports=jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED, --add-opens=java.management/sun.management=ALL-UNNAMED, --add-exports=jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED, -Dio.netty.allocator.maxOrder=11, -Dlog4j2.isThreadContextMapInheritable=true, -Xms1g, -Dlogstash.jackson.stream-read-constraints.max-string-length=200000000, -Djdk.io.File.enableADS=true, -Dfile.encoding=UTF-8, --add-opens=java.base/java.io=ALL-UNNAMED, --add-exports=jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED, -Djruby.compile.invokedynamic=true, -Xmx1g, -Djava.security.egd=file:/dev/urandom, -Djava.awt.headless=true, -Dls.cgroup.cpuacct.path.override=/, -Dls.cgroup.cpu.path.override=/, --add-opens=java.base/sun.nio.ch=ALL-UNNAMED]
[2025-03-12T13:26:34,862][INFO ][logstash.runner          ] Jackson default value override `logstash.jackson.stream-read-constraints.max-string-length` configured to `200000000`
[2025-03-12T13:26:34,863][INFO ][logstash.runner          ] Jackson default value override `logstash.jackson.stream-read-constraints.max-number-length` configured to `10000`
[2025-03-12T13:26:34,877][INFO ][logstash.settings        ] Creating directory {:setting=>"path.queue", :path=>"/usr/share/logstash/data/queue"}
[2025-03-12T13:26:34,885][INFO ][logstash.settings        ] Creating directory {:setting=>"path.dead_letter_queue", :path=>"/usr/share/logstash/data/dead_letter_queue"}
[2025-03-12T13:26:35,385][INFO ][logstash.agent           ] No persistent UUID file found. Generating new UUID {:uuid=>"c0085bc0-9994-4e1b-8dba-c58e4cc80e9b", :path=>"/usr/share/logstash/data/uuid"}
[2025-03-12T13:26:36,616][INFO ][logstash.config.source.local.configpathloader] No config files found in path {:path=>"/usr/share/logstash/config/pipelines/udp_service_logs_es.conf"}
[2025-03-12T13:26:36,617][ERROR][logstash.config.sourceloader] No configuration found in the configured sources.
[2025-03-12T13:26:36,757][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600, :ssl_enabled=>false}
[2025-03-12T13:26:36,777][INFO ][logstash.runner          ] Logstash shut down.
[2025-03-12T13:26:36,788][FATAL][org.logstash.Logstash    ] Logstash stopped processing because of an error: (SystemExit) exit
org.jruby.exceptions.SystemExit: (SystemExit) exit
        at org.jruby.RubyKernel.exit(org/jruby/RubyKernel.java:808) ~[jruby.jar:?]
        at org.jruby.RubyKernel.exit(org/jruby/RubyKernel.java:767) ~[jruby.jar:?]
        at usr.share.logstash.lib.bootstrap.environment.<main>(/usr/share/logstash/lib/bootstrap/environment.rb:90) ~[?:?]
```
![image](https://github.com/user-attachments/assets/bfabc6f3-dd05-47e4-934b-3c4297f2ba7c)


Я пыталась ограничивать "LS_JAVA_OPTS: -Xms512m -Xmx512m", с тем же результатом. Ниже приведены конфигурационные файлы. Если кто знает, в чем тут дело, помогите пожалуйста!!!

---
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
     # LS_JAVA_OPTS: -Xms512m -Xmx512m
    ports:
    - "5044:5044/udp"
    volumes:
    - ./configs/logstash/config.yml:/usr/share/logstash/config/logstash.yml
    - ./configs/logstash/pipelines.yml:/usr/share/logstash/config/pipelines.yml
    - ./configs/logstash/pipelines:/usr/share/logstash/config/logstash
    depends_on:
    - elasticsearch
```
config.yml
```yml
http.host: 0.0.0.0
xpack.monitoring.enabled: false
```
pypelines.yml
```yml
- pipeline.id: service_stamped_json_logs
  pipeline.workers: 1
  pipeline.batch.size: 1
  path.config: "/usr/share/logstash/config/pipelines/udp_service_logs_es.conf"
```
udp_service_logs_es.conf:
```yml
input {
    udp {
        port => "5044"
    }
}
filter {
    grok {
        match => { "message" => "%{IP:client} %{WORD:error} %{NUMBER:code}"}
    }
}
output {
    stdout {
    }
    elasticsearch {
        hosts => [ "${ES_HOST}" ]
    }
}
```
