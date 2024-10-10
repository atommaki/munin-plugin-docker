# munin-plugin-docker

munin plugin to monitor docker, based on python3. It's designed to see overall statistics of your docker environment, not to monitor individual containers.

  * [Metrics](#metrics)
  * [Example graphs](#example_graphs)
  * [Installation](#installation)

## Metrics
<a name="metrics"/>

The plugin uses the [Docker SDK for Python](https://docker-py.readthedocs.io/en/stable/), all metrics comes from the API.
  * containers: number of containers, running and all
  * containers uptime: uptime of the running containers
    * max: uptime of the oldest running container
    * min: uptime of the youngest running container
    * median: median of the uptimes
  * memory: memory usage of the containers
    * total: memory usage of all the containers together (sum(list))
    * max: the highest memory usage of the containers (max(list))
    * avg: average memory usage of the containers (total/n)
  * images: number of images (without intermediate image layers)
  * layerssize: good question, it is what the docker API gives back ([df call](https://docker-py.readthedocs.io/en/stable/api.html#module-docker.api.daemon)) as LayersSize :) Not very well documented, I guess this is the overall size of the images.
  * volumes_n: number of docker volumes
  * volumes_size: used disk space by docker local volumes (`docker system df`)

## Example graphs
<a name="example_graphs"/>

![Number of containers](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-containers.png "Number of containers")

![Containers uptime](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-containers-uptime.png "Containers uptime")

![Containers memory usage](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-memory.png "Containers memory usage")

![Number of images](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-images.png "Number of images")

![Docker LayersSize](https://github.com/atommaki/munin-plugin-docker/raw/master/screenshots/munin-plugin-docker-screenshot-layerssize.png "Docker LayersSize")


## Installation
<a name="installation"/>

### Prerequisites
The plugin requires python3 and some python modules. And of course you already have munin-node and docker on the host.

on Ubuntu 22.04 (or higher):
```
sudo apt install python3 python3-docker
```

### Installing the plugin
 * copy [`docker_stat`](https://raw.githubusercontent.com/atommaki/munin-plugin-docker/master/docker_stat) to the `/usr/share/munin/plugins/` directory:
```
sudo wget https://raw.githubusercontent.com/atommaki/munin-plugin-docker/master/docker_stat -O /usr/share/munin/plugins/docker_stat
```

 * Make it executable:
```
sudo chmod a+x /usr/share/munin/plugins/docker_stat
```

 * create symlinks to it
```
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_containers
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_containers_uptime
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_images
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_memory
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_layerssize
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_volumes_n
sudo ln -s /usr/share/munin/plugins/docker_stat /etc/munin/plugins/docker_stat_volumes_size
```

 * Run one of the scripts to see if all the required python modules are installed:
```
sudo /etc/munin/plugins/docker_stat_memory
```

 * configure munin to run the plugin as `root` user. Add the following lines to `/etc/munin/plugin-conf.d/docker`:
```
[docker*]
user root
```

 * Restart munin-node:
```
sudo systemctl restart munin-node.service
```

