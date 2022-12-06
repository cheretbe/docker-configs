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

#### Version upgrade

* :warning: Review upgrade notes https://manual.seafile.com/docker/upgrade/upgrade_docker/

```shell
# TODO: try https://github.com/containers/skopeo to inspect metadata without pulling
docker pull docker.seadrive.org/seafileltd/seafile-pro-mc:latest
docker image inspect -f '{{ range .ContainerConfig.Env }}{{ println . }}{{ end }}' docker.seadrive.org/seafileltd/seafile-pro-mc:latest | grep SEAFILE_VERSION
# SEAFILE_VERSION=9.0.13
docker pull docker.seadrive.org/seafileltd/seafile-pro-mc:9.0.13
docker image rm docker.seadrive.org/seafileltd/seafile-pro-mc:latest

docker compose stop

sudo zfs snapshot hdd4/seafile@9.0.4_to_9.0.13
zfs list -r -t snapshot hdd4/seafile

# Update version
nano .env

docker compose -f docker-compose.yml -f docker-compose.local.yml config >"local-data/config_$(date +%Y-%m-%d_%H-%M).txt"
docker compose -f docker-compose.yml -f docker-compose.local.yml up --force-recreate --build -d

# May take a long time, use screen
docker exec -it seafile /opt/seafile/seafile-server-latest/seaf-fsck.sh
```
