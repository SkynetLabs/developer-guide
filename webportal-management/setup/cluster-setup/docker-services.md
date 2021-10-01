# Docker Services

## Enable Docker Services

After the initial portal setup you should have the `Sia` docker container running.

{% hint style="info" %}
Sia container, I thought we were running skyd? You are right, we are running skyd. The Docker container is still called Sia for some backwards compatibility but we might fix this in the future. 
{% endhint %}

To launch the rest of the docker services we first want to update our `.env` file that is located in the `skynet-webportal` directory. In the `.env` we are going to enable `accounts` and `jaegar` by setting the `PORTAL_MODULES` variable. 

```text
PORTAL_MODULES=aj
```

{% hint style="info" %}
If you don't want jaegar running you can remove the `j`. Jaegar is our tracing tool used to help debug `skyd`. 
{% endhint %}

Now we can bring up the docker services. We have a helper tool in the `skynet-webportal` repo called `dc` for `docker compose`.  You can bring up all the docker services by running the following command from within the `skynet-webportal` repo.

```text
./dc up -d
```

This will begin the process of downloading all the docker images and building them. Once the process is down you can see the status of the all the services by running:

```text
docker ps
```

This should show all the docker services. Some will be running and some will show as `Restarting`. This is ok and expected. Once we finish the remaining steps those docker services will be online. 

## Elasticsearch

Elasticsearch is part of our ELK stack used to track webportal traffic metrics.

Elastic Search container requires the configuration files to be readable by `elasticsearch` user with `uid:gid` `1000:0`

First stop the container:

```text
docker stop elasticsearch
```

Then set the permissions of the configuration file:

```text
sudo chown -R 1000:1000 docker/data/elasticsearch/data
```

Finally, restart the container:

```text
docker restart elasticsearch
```

### Memory settings

Elasticsearch requires the `vm.max_map_count` kernel setting to be set to at least `262144` for production use.

{% hint style="info" %}
See[ here](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_set_vm_max_map_count_to_at_least_262144) for more information
{% endhint %}

To set actual \(live\) value, execute:

```text
sudo sysctl -w vm.max_map_count=262144
```

To set the value persistently add the following line to `/etc/sysctl.conf` :

```text
sudo vim /etc/sysctl.conf
```

```text
vm.max_map_count=262144
```

To verify the current value, execute:

```text
cat /proc/sys/vm/max_map_count
```

