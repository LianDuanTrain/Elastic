# Install ELK (Elasticsearch, Kibana, and Filebeat) using Docker Compose 
## Topics
- Demo ENV Detail 
- Target ELK version
- Install ELK via Docker Compose
- Explore Nginx Access Log in Kibana

## Demo ENV Detail  
- OS version - Oracle Linux Server release 8.4   
  -`cat /etc/oracle-release`
- Docker Version - 20.10.6  
  -`docker -v`   
- Docker Compose Version - 1.27.4
  - `docker-compose version`  
  - https://docs.docker.com/compose/compose-file/

## Target ELK version
- Version is 7.16.2     
- https://www.docker.elastic.co/  
## Install ELK via Docker Compose 
- `docker-compose up -d`  
- `https://www.composerize.com/`
```
version: '3.8'
services:
    es-node-01:
        container_name: es-node-01
        ports:
            - '9200:9200'
            - '9300:9300'
        environment:
            - discovery.type=single-node
        image: 'docker.elastic.co/elasticsearch/elasticsearch:7.16.2'
        networks:
            - elastic
    kibana-01:
        container_name: kibana-01
        ports:
            - '5601:5601'
        environment:
            - 'ELASTICSEARCH_HOSTS=http://es-node-01:9200'
        image: 'docker.elastic.co/kibana/kibana:7.16.2' 
        healthcheck:
            test: ["CMD", "curl", "-f", "kibana-01:5601"]
            interval: 10s
            timeout: 10s
            retries: 5
        depends_on:
            - es-node-01
        networks:
            - elastic 

    nginxLogGenerator:
        container_name: nginxLogGenerator
        labels:
            - type=nginxLog
        environment:
            - RATE=10
        image: 'kscarlett/nginx-log-generator:sha-5416ec2' 
        depends_on:
            - es-node-01
            - kibana-01
        networks:
            - elastic 

    filebeat:
        user: root
        container_name: filebeat
        command: --strict.perms=false
        volumes:
            - /home/lian/ELK/1 Introduction and Installation/dockerCompose/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro
            - /var/lib/docker/containers:/var/lib/docker/containers:ro
            - /var/run/docker.sock:/var/run/docker.sock:ro
        image: 'docker.elastic.co/beats/filebeat:7.16.2' 
        depends_on:   
             kibana-01:
               condition: service_healthy
    
        networks:
            - elastic              
networks:
  elastic:
    name: elastic      

```  
- https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-reference-yml.html  
## Explore Nginx Access Log in Kibana
- Stack Management
  - Kibana => Index patterns
  - Data => Index Management
- Analytics
  - Discover
  - Dashboard
## Summary  
- Prerequisite Check 
- Target ELK version
- Install ELK Services
- Explore Nginx Access Log in Kibana










