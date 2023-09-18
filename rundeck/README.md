* https://docs.rundeck.com/docs/administration/install/docker.html#open-source-rundeck
* https://docs.rundeck.com/docs/administration/configuration/storage-facility.html#project-storage
    * Since Rundeck 3.4 configuration files for the project (previously stored on-disk) are stored in the database (`/home/rundeck/server/data/grailsdb.mv.db` -> `/opt/docker-data/rundeck/server_data/grailsdb.mv.db`)
* :warning: Docker config reference: https://docs.rundeck.com/docs/administration/configuration/docker.html
* https://github.com/rundeck/rundeck/blob/main/docker/ubuntu-base/Dockerfile
    * https://github.com/rundeck/rundeck/blob/main/docker/official/Dockerfile
    * default credentials are: `admin:admin`
* https://github.com/rundeck/docker-zoo
* https://jwkenney.github.io/ansible-rundeck-integration/
* `2check`, login page forwarding issue: https://github.com/rundeck/rundeck/issues/5175
* :warning: Monitoring: https://docs.rundeck.com/docs/learning/howto/rundeck-exporter.html

It is [recommended](https://docs.rundeck.com/docs/administration/security/authentication.html#propertyfileloginmodule) to use BCRYPT encrypted passwords with realm.properties as it is the most secure option available. Avoid plain, MD5, and CRYPT

```shell
# Workaround for data directory permissions issue and permanent user storage
# https://github.com/rundeck/rundeck/blob/main/docker/official/README.md?plain=1#L65
# [!!] Ansible playbook takes care of this automatically

# As root (before doing 'compose up')
mkdir -p /opt/docker-data/rundeck/server_data
mkdir -p /opt/docker-data/rundeck/config
# admin:password (see 'generating password' below)
echo "admin:BCRYPT:\$2a\$10\$BXlqczm1WOw.rfSfxmBDx.M4dKuN.raHXqF9FzKqCIHmw.0LiYQhy,user,admin" > /opt/docker-data/rundeck/config/realm.properties
# View actual rundeck user ID in the container
# docker compose run --rm --entrypoint /bin/sh rundeck -c 'id -u'
chown 1000 -R /opt/docker-data/rundeck/
# /opt/docker-data/rundeck/realm.properties is a separate mount in compose file

# Generating password hash
docker compose exec -it rundeck java -jar rundeck.war --encryptpwd Jetty
# admin:password
docker compose exec -it rundeck bash -c 'echo "admin:BCRYPT:\$2a\$10\$BXlqczm1WOw.rfSfxmBDx.M4dKuN.raHXqF9FzKqCIHmw.0LiYQhy,user,admin" > /home/rundeck/server/config/realm.properties'
```

```shell
# Interactive debugging
docker compose exec -it rundeck bash
# When service fails to start
docker compose run -it --entrypoint bash rundeck
# Cleanup
docker rm $(docker ps -a -q  --filter ancestor=rundeck/rundeck:4.11.0)
```
