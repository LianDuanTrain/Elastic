# What is Elasticsearch?
## Topics
- What is Elasticsearch?
- What is ELK Stack?

## What is Elasticsearch?
Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases.  
```
curl localhost:9200/foo -XPOST -H 'Content-Type: application/json' -d '{"settings":{"index.number_of_shards": 1}}'
Index data:   
└── [ 102]  elasticsearch  
    └── [ 102]  nodes  
        └── [ 170]  0  
            ├── [ 102]  _state  
            │   └── [ 109]  global-0.st  
            ├── [ 102]  indices  
            │   └── [ 136]  food  
            │       ├── [ 170]  0  
            │       │   ├── .....  
            │       └── [ 102]  _state  
            │           └── [ 256]  state-0.st  
            └── [   0]  node.lock  
```

## What is ELK Stack?
### ELK: Beats,Logstash,Elasticsearch, and Kibana.   
<img src='https://miro.medium.com/max/700/1*cD2gHPbzrrJ4gVqV7iaLvQ.png' >   

### Beats
<img src='https://www.elastic.co/guide/en/beats/libbeat/current/images/beats-platform.png' width="50%">      

### Logstash
<img src='https://www.elastic.co/guide/en/logstash/current/static/images/basic_logstash_pipeline.png' width="50%" >  

### Kibana  
<img src='https://static-www.elastic.co/v3/assets/bltefdd0b53724fa2ce/blt47b86adba2f459aa/5fa31e03bfc5dd7188659491/screenshot-kibana-dashboard-webtraffic2-710-547x308.jpg' width="50%" >  








