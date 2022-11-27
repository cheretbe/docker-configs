* https://github.com/linuxserver/docker-wireguard
* https://github.com/ngoduykhanh/wireguard-ui

Server config
```shell
apt install wireguard
systemctl enable wg-quick@wg0.service
systemctl start wg-quick@wg0.service
```

Client config
```shell
apt install wireguard
# wg-myserver will become the name of network interface
cp client.conf /etc/wireguard/wg-myserver.conf
systemctl enable wg-quick@wg-myserver.service
systemctl start wg-quick@wg-myserver.service
```

```shell
wg show

# Debug logging (as root)
modprobe wireguard
echo module wireguard +p > /sys/kernel/debug/dynamic_debug/control
journalctl -kf

# remove module
modprobe -r wireguard
lsmod | grep wireguard
```

* https://www.man7.org/linux/man-pages/man5/systemd.path.5.html

```shell
cat > /etc/systemd/system/wgui-config.path<< EOF
[Unit]
Description=Watch /etc/wireguard/wg0.conf for changes

[Path]
PathModified=/etc/wireguard/wg0.conf

[Install]
WantedBy=multi-user.target
EOF

cat > /etc/systemd/system/wgui-config.service<< EOF
[Unit]
Description=Restart Wireguard on configuration change
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/bin/systemctl restart wg-quick@wg0.service

[Install]
RequiredBy=wgui-config.path
EOF

systemctl enable wgui-config.{path,service}
systemctl start wgui-config.{path,service}
```