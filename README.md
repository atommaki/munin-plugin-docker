# munin-plugin-docker

munin plugin to monitor docker

## Installation
### Prerequisites
The plugin requires python3 and some python modules. You can install them on Ubuntu 18.04:
```
sudo apt install python3 python3-pip
sudo python3 -m pip install docker
```
And of course you already have munin-node on the host.

### Installing the plugin
copy `docker_stat` to the `/usr/share/munin/plugins/` directory


create symlinks to it
```
ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_containers
ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_images
ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_memory
ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_layerssize
```

Run on of the scripts to see if all the required python modules are installed:
```
/etc/munin/plugins/docker_stat_memory
```


configure munin to run the plugin as root:
```
cat <<EOF > /etc/munin/plugin-conf.d/docker
[docker*]
user root
EOF
```

Restart munin-node:
```
systemctl restart munin-node.service
```

