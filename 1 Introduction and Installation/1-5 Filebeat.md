# Filebeat 


## Topics
- What is Filebeat?
- Filebeat Components
- Filebeat Workflow

### What is Filebeat?
Beats are lightweight data  collection  agent beacse it is based Go lunange. 
### Filebeat Components
<img src='Filebeat.jpg'>  


- Crawler 
- Input 
- Harvester
- Module 
- Registrar
- Libbeat
  - Publisher 
    - Client
    - Processor
    - Queue
    - Output
    - Acker 
  - Autodiscover
    - https://www.elastic.co/guide/en/beats/filebeat/current/configuration-autodiscover.html


###  Filebeat Workflow
1. Crawler    
  <img src='step1.jpg'>     
2. Input  
  <img src='step2.jpg'>  
3. Registrar   
 <img src='step4.jpg'>      
4. Harvester   
  <img src='step3.jpg'>   
 
5. Module  
 <img src='step5.jpg'>  
6. Publisher Client   
 <img src='step6.jpg'>   
7. Processor     
  <img src='step6-1.jpg'> 
8. Queue   
  <img src='step6-2.jpg'>     
9. Output   
 <img src='step8.jpg'>   
10. Acker    
  <img src='step7.jpg'>    

  



## Summary  
- What is Filebeat?
- Filebeat and Libbeat Components
- Filebeat Workflow






