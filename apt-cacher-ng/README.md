* https://github.com/growlf/docker-apt-cacher-ng

```shell
APT_PROXY=10.0.2.2
cat <<-EOF >/etc/apt/apt.conf.d/02proxy
  Acquire::http::proxy "http://${APT_PROXY}:3142";
  Acquire::ftp::proxy "${APT_PROXY}:3142";
EOF
```
