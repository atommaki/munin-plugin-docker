# munin-plugin-docker

munin plugin to monitor docker, based on python3. It's designed to see overall statistics of your docker environment, not to monitor individual containers.

[Metrics](## Metrics)
[Example graphs](## Example graphs)
[Installation](## Installation)

## Metrics

The plugin uses the [Docker SDK for Python](https://docker-py.readthedocs.io/en/stable/), all metrics comes from the API.
  * containers: number of containers, running and all
  * memory: memory usage (RSS) of the containers
  * images: number of images (without intermediate image layers)
  * layerssize: good question, it is what the docker API gives back ([df call](https://docker-py.readthedocs.io/en/stable/api.html#module-docker.api.daemon)) as LayersSize :) Not very well documented, I guess this is the overall size of the images.

## Example graphs

![Number of containers](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-containers.png "Number of containers")

![Containers memory usage](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-containers.png "Containers memory usage")

![Number of images](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-images.png "Number of images")

![Docker LayersSize](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-containers.png "Docker LayersSize")


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

Make it executable:
```
chmod a+x /usr/share/munin/plugins/docker_stat
```

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

