* https://gitlab.com/gitlab-org/omnibus-gitlab/tree/master/docker
* https://www.czerniga.it/2021/11/14/how-to-install-gitlab-using-docker-compose/
* https://www.danieleagle.com/2017/01/gitlab-ce-with-https-using-docker/
    * https://github.com/danieleagle/gitlab-https-docker/blob/master/docker-compose.yml
* :warning: Running GitLab in a memory-constrained environment: https://docs.gitlab.com/omnibus/settings/memory_constrained_envs.html
    * deprecated settings
    ```
    gitaly['cgroups_count'] has been deprecated since 15.1 and will be removed in 16.0. Use `gitaly['cgroups_repositories_count']` instead.
    gitaly['cgroups_memory_enabled'] has been deprecated since 15.1 and will be removed in 16.0. Use `gitaly['cgroups_memory_bytes'] or gitaly['cgroups_repositories_memory_bytes'] instead.
    gitaly['cgroups_cpu_enabled'] has been deprecated since 15.1 and will be removed in 16.0. Use `gitaly['cgroups_cpu_shares'] or gitaly['cgroups_repositories_cpu_shares'] instead.
    ```
```shell
# Configuration
# [!] GITLAB_OMNIBUS_CONFIG in compose file will take precedence over
#     /etc/gitlab/gitlab.rb in the container
docker exec -it gitlab /bin/bash
vi /etc/gitlab/gitlab.rb
gitlab-ctl reconfigure

# View actual GITLAB_OMNIBUS_CONFIG setting
docker inspect -f '{{ .Config.Env }}' gitlab

# View generated password (user name is 'root')
docker exec gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

```

`docker-compose.local.yml` example:
```yaml
services:
  gitlab:
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://something'
        nginx['listen_https'] = true
    # 4Gb with no swap
    mem_limit: 4g
    memswap_limit: 4g
```
