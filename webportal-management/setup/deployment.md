# Deployment

## Overview

At the end of this section, you will have your new functioning portal in production.

## Deploy

Now that you have gone through the setup for the portal, the last step is to run the [ansible deploy script](https://github.com/SkynetLabs/ansible-playbooks#deploy-skynet-webportal) on the portal to verify that you can successfully deploy updates to your portal. On successful completion of the deploy script your portal will be ready for production.

## Add To Production

After successfully running the deploy script, you can enable your new portal in your DNS load balancer.

## Host.ini

Lastly, make sure to update your `hosts.ini` file accordingly so that your new server is included in future ansible deployments and updates.  



 

