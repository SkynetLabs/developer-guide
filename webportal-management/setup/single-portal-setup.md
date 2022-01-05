# Single Portal Setup

## Overview

At the end of this section, you will have a running `skyd` instance that is ready to form contracts once you send it siacoins.

## Prerequisites

The only prerequisite for this step is to define the version of the `skyd`, `skynet-webportal`, and `skynet-accounts` repos that you wish to install in your `portal-versions.yml` file.

**Example**&#x20;

`ansible-playbooks/my-vars/portal-versions.yml`

```
---
# Set skynet-webportal, skyd, accounts versions by entering git branch, tag or
# commit
portal_repo_version: "deploy-2021-08-24"
portal_skyd_version: "deploy-2021-08-24"
portal_accounts_version: "deploy-2021-08-09"
```

## Portal Setup Following

Once your `portal-versions.yml` file is ready, it is time to run the [portal-setup-following.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-following) script and don't forget to add the `portal-versions.yml` as a parameter.

**Example**

&#x20;`./scripts/portals-setup-following.sh -e @my-vars/portal-versions.yml --limit <server name>` executed from the `ansible-playbooks` directory.

{% hint style="info" %}
**NOTE** the first run of this script will timeout and fail. This is expected. Please see the below sections on options to handle completing this step.
{% endhint %}

### Bootstrapping Consensus

When you run the [portal-setup-following.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-following) script the first time it will timeout and fail. This is because part of the script is waiting for consensus to be synced. For those familiar with running a `skyd` node, you will note that this can take several hours, so the timeout in the script is expected. When the script times out, the `skyd` node is running. You can check with this:

```
docker ps
```

This should output that the `sia` container is running.&#x20;

{% hint style="info" %}
**NOTE** the `sia` and `siad` references in the Dockerfile are for compatibility from when Skynet Labs and the Sia Foundation split.&#x20;
{% endhint %}

At this point, since the node is running, you can either let it run and check back later when it is synced, or you can copy over a bootstrapped `consensus.db`. If this is your first portal, you should probably just let your node sync on its own. However, for all other portals, you can take a `consensus.db` from a previous node and copy it to your new server. Once you bootstrapped `consensus.db` is copied to your server you can copy it into the docker data directory with the following command:

```
# First make sure the container is stopped
docker stop sia

# Copy consensus.db into the docker data directory
docker cp /path/to/consensus.db sia:/sia-data/consensus/consensus.db
```

Now that you've copied your bootstrapped `consensus.db` is in the docker data directory you can re-run the [portal-setup-following.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-following) script.&#x20;

## Fund The Portal

You now have a running `skyd` node that is ready to start forming contracts. To fund the the node, generate a wallet address.

```
docker exec sia siac wallet address
```

Now you can send siacoins to this address to fund your node. It is recommended to fund the wallet with 3x your allowance funds.&#x20;
