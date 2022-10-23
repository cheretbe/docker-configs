* Find out username/password for `docker.seadrive.org`: https://customer.seafile.com/downloads/
```shell
# [!!] Without this elasticsearch container will not work
mkdir -p /opt/docker-data/seafile/elasticsearch/data
chmod 777 -R /opt/docker-data/seafile/elasticsearch/data


# HTTPS version with Let's Encrypt certificate
docker compose -f docker-compose.yml -f docker-compose.https.yml up -d
docker compose -f docker-compose.yml -f docker-compose.https.yml up --force-recreate --build -d

docker logs seafile
docker logs seafile-elasticsearch
docker logs seafile-mysql
docker logs seafile-memcached
```

Custom override file (`docker-compose.local.yml`) example:
```yaml
services:
  seafile:
    ports:
      - "80:80"
      - "12345:443"
```
