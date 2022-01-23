# Filebeat Ship App Log to Elasticsearch


## Topics
- How to Generate App Log?
- Analysis App Log Format Pattern
- Configure Filebeat to Process App Log
- Explore App Log in Kibana


## How to Generate App Log?
- Selenium
- JMeter
- OWASP ZAP
- App Log Generate
  - https://hub.docker.com/r/febbweiss/java-log-generator


## Analysis App Log Format Pattern
- docker run --name javaLogGenerator --label type=javaAppLog -d --rm febbweiss/java-log-generator

```
17-01-2022 11:24:22.028 [pool-16-thread-1] ERROR com.github.vspiewak.loggenerator.ExceptionRequest - Unexpected error
java.lang.RuntimeException: null
        at com.github.vspiewak.loggenerator.ExceptionRequest.run(ExceptionRequest.java:12) [log-generator-0.0.2.jar:na]
        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511) [na:1.8.0_131]
        at java.util.concurrent.FutureTask.run(FutureTask.java:266) [na:1.8.0_131]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142) [na:1.8.0_131]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617) [na:1.8.0_131]
        at java.lang.Thread.run(Thread.java:748) [na:1.8.0_131]
17-01-2022 11:24:22.028 [pool-16-thread-1] INFO com.github.vspiewak.loggenerator.SellRequest - id=239,ip=86.204.108.183,email=client240@gmail.com,sex=M,brand=Apple,name=Macbook Pro Rétina,model=Macbook Pro Rétina - Ecran 15 - Core i7 Quad - Ram 16Go,category=Portable,options=Ecran 15|Core i7 Quad|Ram 16Go,price=2779.0
17-01-2022 11:24:22.028 [pool-16-thread-1] INFO com.github.vspiewak.loggenerator.SearchRequest - id=240,ip=78.223.176.115,brand=Apple,name=iPhone 5S,model=iPhone 5S - Argent - Disque 16Go,category=Mobile,color=Argent,options=Disque 16Go,price=699.0
17-01-2022 11:24:23.028 [pool-17-thread-1] INFO com.github.vspiewak.loggenerator.SearchRequest - id=241,ip=90.33.93.13,category=Mobile
17-01-2022 11:24:23.028 [pool-17-thread-1] INFO com.github.vspiewak.loggenerator.SellRequest - id=242,ip=90.37.80.119,email=client243@gmail.com,sex=M,brand=Apple,name=Mac Mini,model=Mac Mini - Core i7,category=Ordinateur,options=Core i7,price=829.0

```

## Configure Filebeat to Process App Log
### Auto Discover Container Log   

```
filebeat.autodiscover:
  providers:
    - type: docker
      hints.enabled: true
      templates:
      - condition:
            equals:
              docker.container.labels.type: javaAppLog
        config:
          - type: container
            multiline.type: pattern
            multiline.pattern: '^[[:space:]]+(at|\.{3})[[:space:]]+\b|^java.lang.RuntimeException:'
            multiline.negate: false
            multiline.match: after
            paths:
              - /var/lib/docker/containers/${data.docker.container.id}/*.log

```



###  Handle Multiline Exception

```
multiline.type: pattern
multiline.pattern: '^[[:space:]]+(at|\.{3})[[:space:]]+\b|^java.lang.RuntimeException:'
multiline.negate: false
multiline.match: after

```
https://www.elastic.co/guide/en/beats/filebeat/current/multiline-examples.html

###  How to Use Processors?
- Libbeat 
   - Publisher   
      - Client=>Processor=>Queue=>Output
```
- add_fields:
    target: container
    fields:
      env: dev
- add_locale:
      format: abbreviation
     
- add_host_metadata: ~      

- drop_fields:
      fields: ["host.name", "ecs.version", "agent.version", "agent.type", "agent.id", "agent.ephemeral_id", "agent.hostname", "input.type"]

```
### Demo

`docker-compose up -d`

## Explore App Log in Kibana
- Stack Management
  - Kibana => Index patterns
  - Data => Index Management
- Analytics
  - Discover



## Summary  
- How to Generate App Log?
- Analysis App Log Format Pattern
- Configure Filebeat to Process App Log 
- Explore App Log in Kibana  



