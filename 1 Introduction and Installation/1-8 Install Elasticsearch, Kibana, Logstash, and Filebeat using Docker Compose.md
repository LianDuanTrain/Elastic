# Install Elasticsearch, Kibana, Logstash, and Filebeat using Docker Compose
## Topics
- Demo ENV Detail 
- Target ELK version
- Demo ELK System Overview
- Install ELK via Docker Compose
- Explore Java Apps Log in Kibana

## Demo ENV Detail  
- OS version - Oracle Linux Server release 8.4   
  -`cat /etc/oracle-release`
- Docker Version - 20.10.6  
  -`docker -v`   
- Docker Compose Version - 1.27.4
  - `docker-compose version`  
  - https://docs.docker.com/compose/compose-file/

## Target ELK version
- Version is 7.17.0    
- https://www.docker.elastic.co/  

## Demo ELK System Overview

<img src='./InstallElasticsearchKibanaLogstashFilebeatusingDockerComposeImage/Install-Elasticsearch-Kibana-Logstash-Filebeat-using-Docker-Compose.gif'> 

## Install ELK via Docker Compose 


```
version: '3.8'
services:
# Elasticsearch service  
    es-node-01:
        container_name: es-node-01
        ports:
            - '9200:9200'
            - '9300:9300'
        environment:
            - discovery.type=single-node
        image: 'docker.elastic.co/elasticsearch/elasticsearch:7.17.0'
        networks:
            - elastic
# Kibana service              
    kibana-01:
        container_name: kibana-01
        ports:
            - '5601:5601'
        environment:
            - 'ELASTICSEARCH_HOSTS=http://es-node-01:9200'   
        image: 'docker.elastic.co/kibana/kibana:7.17.0'   
        healthcheck:
            test: ["CMD", "curl", "-f", "kibana-01:5601"]
            interval: 50s
            timeout: 50s
            retries: 5
        depends_on:
            - es-node-01
        networks:
            - elastic 
# Java App service
    javaApp:
        image: 'febbweiss/java-log-generator:latest' 
        depends_on:
            - es-node-01
            - kibana-01
        networks:
            - elastic 
# Logstash service 
    logstash:
        container_name: logstash-01
        volumes:
            - /home/lian/ELK/1 Introduction and Installation/dockerComposeEKLF/logstash.conf:/usr/share/logstash/pipeline/logstash.conf:ro 
            - /home/lian/ELK/1 Introduction and Installation/dockerComposeEKLF/logstash.yaml:/usr/share/logstash/config/logstash.yml:ro 
        image: 'docker.elastic.co/logstash/logstash:7.16.3'
        depends_on:   
             kibana-01:
               condition: service_healthy  
        networks:
            - elastic 

# Filebeat service 
    filebeat:
        user: root
        container_name: filebeat-01
        command: --strict.perms=false
        volumes:
            - /home/lian/ELK/1 Introduction and Installation/dockerComposeEKLF/filebeat.docker.yaml:/usr/share/filebeat/filebeat.yml:ro
            - /var/lib/docker/containers:/var/lib/docker/containers:ro
            - /var/run/docker.sock:/var/run/docker.sock:ro
        image: 'docker.elastic.co/beats/filebeat:7.16.3' 
        depends_on:   
             kibana-01:
               condition: service_healthy
        networks:
            - elastic              
networks:
  elastic:
    name: elastic      

```  

- `docker-compose up -d`   
- `docker-compose up -d --scale javaApp=3`   


## Explore Java Apps Log in Kibana
- Stack Management
  - Kibana => Index patterns
  - Data => Index Management
- Analytics
  - Discover
  - Dashboard
## Summary  
- Demo ENV Detail 
- Target ELK version
- Demo ELK System Overview
- Install ELK via Docker Compose
- Explore Java Apps Log in Kibana














