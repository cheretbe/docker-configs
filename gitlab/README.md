* https://gitlab.com/gitlab-org/omnibus-gitlab/tree/master/docker
* https://www.czerniga.it/2021/11/14/how-to-install-gitlab-using-docker-compose/
* https://www.danieleagle.com/2017/01/gitlab-ce-with-https-using-docker/
    * https://github.com/danieleagle/gitlab-https-docker/blob/master/docker-compose.yml

```shell
# Configuration
# [!] GITLAB_OMNIBUS_CONFIG in compose file will take precedence over
#     /etc/gitlab/gitlab.rb in the container
docker exec -it gitlab /bin/bash
vi /etc/gitlab/gitlab.rb
gitlab-ctl reconfigure
```
