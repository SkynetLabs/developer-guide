# Retiring Webportal Servers

Two reasons one might need to retire a webportal server:

1. Scaling limitations
2. Takedown requests

## Scaling Limitations

One of the core processes that makes Skynet possible, and is a key component of the webportal stack, is the `skyd` daemon. There are a few known limitations of the `skyd` daemon that have been observed that can impact the performance of the webportal. There limitations are:

1. Contract Size &gt; 100TB
2. Repair Data &gt; 10 TB
3. Total Number of Files &gt; 1.5 million

{% hint style="info" %}
The Repair Data should be temporary. Retiring the server should allow the server to catch back up. If this is not happening, further investigation is required to ensure the server can manage the load and keep the data healthy on the network. 
{% endhint %}

If one of these conditions is met, it is recommended to retire the server and put it in maintenance mode. This means simply removing it from your Load Balancer so that it is not serving new traffic, but is still running and maintaining the data it has pinned. 

To do this, you simply must add the server to the `out_of_LB` group in your `hosts.ini` file. This will allow you to still include the server in upgrades, but it will not enable the `health-check` which means it will not be added back into the Load Balancer.

If the server is ready to come out of maintenance mode, because the scalability was improved or the repair data is back under control, just remove it from the `out_of_LB` group and run the deploy or restart script. 

## Takedown Requests

Some takedown requests are not immediately resolvable. In order to comply with a hosting provider and avoid them turning off access to the server, you can use the [takedown-skynet-webportal.sh](https://github.com/SkynetLabs/ansible-playbooks#takedown-skynet-webportal) script. This will shut down all services except `skyd`, which will prevent external traffic but allow the `skyd` daemon to keep the data healthy on the network. 

When the issue is resolved and the server is ready to be brought back up, simply remove the server from the `webportals_takedown` group and run the deploy or restart script.



