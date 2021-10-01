---
description: Below are the requirements for a Public Web Portal server.
---

# Requirements

## Hardware

The following are the recommendations for hardware requirements. 

### CPU

Intel Xeon E-2136 or AMD Ryzen 5 3600 CPU or better.

### RAM

32GB of ram or more.

### Storage

500GB SSD or more. SSD is required, disk space will be most impacted by the amount of logging the server does. 

## Network

1 Gbps up and down is recommended. 

High limit or unlimited bandwidth is encouraged. Bandwidth usage is of course highly dependant on the usage of each portal but it is important to note, that even with low numbers of new uploads, repair bandwidth will continue to increase over time. 

## System OS

It is recommended that all webportals run Debian 10.

## Applications

### LastPass

Currently, a number of the ansible scripts use LastPass for managing server credentials and common files. If you plan to use the ansible scripts it is currently required to have a LastPass account.

## DNS

The first DNS related requirement is that you own a domain for your webportal. If you are just setting up a single portal then this is really all you need.  However, is it recommended to have at least two portals that are load balanced behind your domain so that you can do upgrades and other maintenance without causing downtime to users.

Each DNS set up will be slightly different depending on the provider you are using. In general, the set up for a multi-portal configuration, or cluster, will look something like the following:

* Main domain, i.e. `siasky.net`
* A records for individual servers/portals, i.e `portal1.siasky.net`
* Load balancer \(sometimes called traffic policy\) for the main domain that splits traffic to the individual portals.

### Health Checks

Part of the webportal infrastructure is a set of health checks. These health checks can be seen at `/health-check`. It is encouraged to set up health check checks with your provider so if a server is having issues and failing health checks it is removed from your load balancer. 

**Health Check url:** `https://<portal name>.domain/health-check` i.e. `https://portal1.siasky.net/health-check`. 

These health checks are accessed via Port 443.

