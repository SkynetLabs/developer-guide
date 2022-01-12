# Accounts

## Overview

At the end of this section, you will have Accounts enabled on your portal.&#x20;

## Enable Docker Services

After the initial portal setup, you should have the `sia` and `mongo` docker container running.

{% hint style="info" %}
Sia container, I thought we were running skyd? You are right, we are running skyd. The Docker container is still called Sia for some backwards compatibility but we might fix this in the future.&#x20;
{% endhint %}

There are several other docker services that the portal can run. To launch these docker services, simply update your `PORTAL_MODULES` variable in the `.env` file that is located in the `skynet-webportal` directory. The options are documented in the `.env` file, but here is an example of the options:

```
# Possible choices:
# - 'a': Accounts (https://github.com/SkynetLabs/skynet-accounts)
# - 'b': Blocker (https://github.com/SkynetLabs/blocker)
# - 'j': Jaeger
# - 'm': MongoDB (when 'a' (i.e. Accounts) are used, this is included automatically)
# - 's': Malware Scanner (https://github.com/SkynetLabs/malware-scanner)
# - 'u': Abuse Scanner (https://github.com/SkynetLabs/abuse-scanner)
```

For most portals, it is recommended to run all the services but Jaegar. Jaegar is a debugging service, so doesn't need to be run unless you plan on debugging the portal code itself. So the `PORTAL_MODULES` in the `.env` file should be updated to the following:

```
PORTAL_MODULES=absu 
```

In order to use accounts, the portal will need to issue and sign [JWT](https://auth0.com/docs/secure/tokens/json-web-tokens)s. For that to be possible, we need a [JWKS](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets) defined on each server. For the moment, this is a manual operation. The instructions for generating such a file are here: [https://github.com/SkynetLabs/skynet-accounts#generating-a-jwks](https://github.com/SkynetLabs/skynet-accounts#generating-a-jwks). Once you generate the content, put it under `/home/user/skynet-webportal/docker/data/accounts/conf/jwks.json`. Put the same file on each server in the cluster.
