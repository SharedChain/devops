# Run a Local ELK Stack
Eleastic Search - Logstash - Kibana

## Pull the Image
```sudo docker pull sebp/elk```

## Run
```sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -p 9300:9300 -it --name elk sebp/elk```
