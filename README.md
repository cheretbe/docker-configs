# docker-configs

```shell
docker compose config >"local-data/config_$(date +%Y-%m-%d_%H-%M).txt"
docker compose -f docker-compose.yml -f docker-compose.local.yml config >"local-data/config_$(date +%Y-%m-%d_%H-%M).txt"

docker compose config | colordiff - local-data/config_2022-11-16_12-47.txt
docker compose -f docker-compose.yml -f docker-compose.local.yml config | colordiff - local-data/config_2022-11-16_12-47.txt

docker compose ls --all

cd /opt/docker-files/project
docker compose ps --all
docker compose images

docker inspect -f '{{ index .Config.Labels "com.docker.compose.project.config_files" }}' container-name

# Using custom override file
# [!] Use config to inspect resulting config
docker compose -f docker-compose.yml -f docker-compose.local.yml config
docker compose -f docker-compose.yml -f docker-compose.local.yml up -d
docker compose -f docker-compose.yml -f docker-compose.local.yml up --force-recreate --build -d
```

Custom override file (docker-compose.local.yml) example:

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
