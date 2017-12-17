# Run a Local ELK Stack
Eleastic Search - Logstash - Kibana

## Pull the Image
```sudo docker pull sebp/elk```

## Run - First time
```sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -p 9300:9300 -it --name elk sebp/elk```

## Stop
```^C```

## Restart
```sudo docker-compose up elk```

## In Docker Compose
```elk:
  image: sebp/elk
  ports:
    - "5601:5601"
    - "9200:9200"
    - "5044:5044
```

## Connect to the container
```
sudo docker exec -it <container-name> /bin/bash
```

## Creating logs
Kibana needs these for the UI to work.




## View Kibana
```http://localhost:5601```
