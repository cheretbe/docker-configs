# docker-configs

```shell
docker compose ls --all

cd /opt/docker-files/project
docker compose ps --all
docker compose images

docker inspect -f '{{ index .Config.Labels "com.docker.compose.project.config_files" }}' container-name

# Using custom override file
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
