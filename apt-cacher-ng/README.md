* https://github.com/growlf/docker-apt-cacher-ng
* apt-cacher-ng cannot act as MITM proxy for SSL repos. Remapping of a virtual http repository to actual https one needs to be configured.
   * https://blog.packagecloud.io/using-apt-cacher-ng-with-ssl-tls/
   * https://askubuntu.com/questions/1307210/how-to-get-apt-cacher-ng-to-download-and-cache-packages-from-apt-https-repositor/1307211#1307211 

```shell
APT_PROXY=10.0.2.2
cat <<-EOF >/etc/apt/apt.conf.d/02proxy
  Acquire::http::proxy "http://${APT_PROXY}:3142";
  Acquire::ftp::proxy "${APT_PROXY}:3142";
EOF
```
