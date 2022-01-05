---
description: Below are the prerequisites for setting up a Public Web Portal server.
---

# Prerequisites

## DNS

The first DNS related requirement is that you own a domain for your webportal. If you are just setting up a single portal then this is really all you need.  However, is it recommended to have at least two servers that are load balanced behind your domain so that you can do upgrades and other maintenance without causing downtime to users.

Each DNS set up will be slightly different depending on the provider you are using. In general, the set up for a multi-server configuration, or cluster, will look something like the following:

* Main domain, i.e. `siasky.net`
* A records for individual servers/portals, i.e `server1.siasky.net`
* Load balancer (sometimes called traffic policy) for the main domain that splits traffic to the individual servers.

### Health Checks

Part of the webportal infrastructure is a set of health checks. These health checks can be seen at `/health-check`. It is encouraged to set up health check checks with your provider so if a server is having issues and failing health checks it is removed from your load balancer.&#x20;

**Health Check URL:** `https://<server name>.domain/health-check` i.e. `https://server1.siasky.net/health-check`.&#x20;

These health checks are accessed via Port 443.
