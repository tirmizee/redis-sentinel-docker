# redis-sentinel-docker

### docker-compose with no redis password

```yaml

version: '3.7'

services:
  redis-master:
    image: redis
    command: redis-server --appendonly yes
    ports:
      - "6379:6379"
    networks:
      redis-net:
        ipv4_address: 172.20.0.2

  redis-replica1:
    image: redis
    command: redis-server --appendonly yes --slaveof redis-master 6379
    ports:
      - "6380:6379"
    networks:
      redis-net:
        ipv4_address: 172.20.0.3
    depends_on:
      - redis-master

  redis-replica2:
    image: redis
    command: redis-server --appendonly yes --slaveof redis-master 6379
    ports:
      - "6381:6379"
    networks:
      redis-net:
        ipv4_address: 172.20.0.4
    depends_on:
      - redis-master

  redis-sentinel1:
    image: redis
    command: redis-sentinel /data/sentinel.conf
    ports:
      - "26379:26379"
    volumes:
      - /Users/pratyayeekhaday/Desktop/Github Project/redis-sentinel-docker/sentinel1:/data
    networks:
      redis-net:
        ipv4_address: 172.20.0.5

  redis-sentinel2:
    image: redis
    command: redis-sentinel /data/sentinel.conf
    ports:
      - "26380:26379"
    volumes:
      - /Users/pratyayeekhaday/Desktop/Github Project/redis-sentinel-docker/sentinel2:/data
    networks:
      redis-net:
        ipv4_address: 172.20.0.6

  redis-sentinel3:
    image: redis
    command: redis-sentinel /data/sentinel.conf
    ports:
      - "26381:26379"
    volumes:
      - /Users/pratyayeekhaday/Desktop/Github Project/redis-sentinel-docker/sentinel3:/data
    networks:
      redis-net:
        ipv4_address: 172.20.0.7

networks:
  redis-net:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16

```

### run 

    docker compose up 

### down master

    docker compose stop redis-master


### track sentinel log

    docker compose logs redis-sentinel1


### sentinel commads

    INFO REPLICATION

    SENTINEL get-master-addr-by-name mymaster

