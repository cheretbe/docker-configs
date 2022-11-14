* Find out username/password for `docker.seadrive.org`: https://customer.seafile.com/downloads/

```shell
# [!!] Without this elasticsearch container will not work
mkdir -p /opt/docker-data/seafile/elasticsearch/data
chmod 777 -R /opt/docker-data/seafile/elasticsearch/data
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
