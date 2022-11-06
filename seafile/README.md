* Find out username/password for `docker.seadrive.org`: https://customer.seafile.com/downloads/
```shell
# [!!] Without this elasticsearch container will not work
mkdir -p /opt/docker-data/seafile/elasticsearch/data
chmod 777 -R /opt/docker-data/seafile/elasticsearch/data


# Using custom override file
docker compose -f docker-compose.yml -f docker-compose.local.yml up -d
docker compose -f docker-compose.yml -f docker-compose.local.yml up --force-recreate --build -d

docker logs seafile
docker logs seafile-elasticsearch
docker logs seafile-mysql
docker logs seafile-memcached
```

Custom override file (`docker-compose.local.yml`) example:
```yaml
services:
  db:
    ports:
      - "3306:3306"
  seafile:
    ports:
      - "80:80"
      - "12345:443"
```
