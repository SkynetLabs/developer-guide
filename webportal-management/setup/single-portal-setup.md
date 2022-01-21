# Single Portal Setup

## Overview

At the end of this section, you will have a running `skyd` instance that is ready to form contracts once you send it siacoins and a running `mongo` container.

This can be confirmed by running the `docker ps` command on your server and checking that it looks like the following:

```
$ ssh user@mydomain.com
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED              STATUS              PORTS                                           NAMES
a3614009e7b7   skynet-webportal_sia   "./run.sh"               About a minute ago   Up About a minute   9980/tcp                                        sia
7cde488f0fd0   mongo:4.4.1            "docker-entrypoint.s…"   3 days ago           Up 3 days           0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   mongo
```

## Prerequisites

### Ansible config.yml

The only prerequisite for this step is to define the version of the `skyd`, `skynet-webportal`, and `skynet-accounts` repos that you wish to install in your `config.yml` file.

**Example**&#x20;

`ansible-playbooks/my-vars/config.yml`

```
---
# Set skynet-webportal, skyd, accounts versions by entering git branch, tag or
# commit
portal_repo_version: "deploy-2021-12-21"
portal_skyd_version: "deploy-2021-12-21"
portal_accounts_version: "deploy-2021-12-21"
```

### Mongo Settings in Lastpass

{% hint style="warning" %}
This LastPass section should be removed and updated to a `config.yml` section once the .env file generate is updated and handles all yml files. These fields should be input vars in the `config.yml` file and the mgkey should be saved in lastpass but then ansible should copy it into the yml file. Also ansible should be able to generate the mgkey automatically if one doesn't exist.
{% endhint %}

{% hint style="info" %}
When creating your `cluster-prod.yml` file, keep in mind that the `prod` suffix in the name is the id of your cluster, as defined by the `portal_cluster_id=prod` entry in your hosts.ini file, located in the `ansible-private` repository. Please see [these docs](https://github.com/SkynetLabs/ansible-playbooks#requirements) for details.
{% endhint %}

In order for mongo to start, we need to set some fields in our `cluster-prod.yml` file. &#x20;

#### Optional

The following fields can be set manually, or if they are left blank, ansible will use the defaults and autogenerate a strong password.

```
skynet_db_user: admin
skynet_db_pass: <strong password>
skynet_db_replicaset: skynet 
```

If you choose to manually set the `skynet_db_pass,` make sure it is set to a strong password, like one generated from [https://passwordsgenerator.net/](https://passwordsgenerator.net).

#### Required

Next, we need to generate a keyfile for mongodb. Generate a `mgkey` by running the following command:

```
openssl rand -base64 756 > mgkey
sed -e 's/^/  /' mgkey > mgkey_yml
```

This will create two files, a `mgkey` and a `mgkey_yml` file.  The `mgkey_yml` file is the same as the `mgkey` file but with two spaces prepended to each line. These spaces are for proper `yml` formatting.

Example:

```
$ cat mgkey
asdf
asdf
asdf
$ cat mgkey_yml
  asdf
  asdf
  asdf
```

Open the `mgkey` file, and copy its contents into LastPass as a secure note titled `mgkey`.

Open the `mgkey_yml` file, and copy its contents into the `cluster-prod.yml` file under `mongo_db_mgkey`.

```
mongo_db_mgkey: |
  asdf
  asdf
  asdf
```

## Enable Docker Services

After the initial portal setup, you should have the `sia` and `mongo` docker container running.

{% hint style="info" %}
Sia container, I thought we were running skyd? You are right, we are running skyd. The Docker container is still called Sia for some backwards compatibility but we might fix this in the future.&#x20;
{% endhint %}

There are several other docker services that the portal can run. To launch these docker services, simply update your `PORTAL_MODULES` variable in the `<server>.yml` file in LastPass. The options are documented [here](https://github.com/SkynetLabs/ansible-playbooks/blob/master/playbooks/templates/.env.j2#L37), but here is an example of the options:

```
# Possible choices:
# - 'a': Accounts (https://github.com/SkynetLabs/skynet-accounts)
# - 'b': Blocker (https://github.com/SkynetLabs/blocker)
# - 'j': Jaeger
# - 'm': MongoDB (when 'a' (i.e. Accounts) are used, this is included automatically)
# - 's': Malware Scanner (https://github.com/SkynetLabs/malware-scanner)
# - 'u': Abuse Scanner (https://github.com/SkynetLabs/abuse-scanner)
```

For most portals, it is recommended to run all the services but Jaegar. Jaegar is a debugging service, so doesn't need to be run unless you plan on debugging the portal code itself. So the `PORTAL_MODULES` should be updated to the following:

```
PORTAL_MODULES=abs
```

### JWTs

In order to use accounts, the portal will need to issue and sign [JWT](https://auth0.com/docs/secure/tokens/json-web-tokens)s. For that to be possible, we need a [JWKS](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets) defined on each server. For the moment, this is a manual operation. The instructions for generating such a file are here: [https://github.com/SkynetLabs/skynet-accounts#generating-a-jwks](https://github.com/SkynetLabs/skynet-accounts#generating-a-jwks). Once you generate the content, save it in your Lastpass folder and name it `jwks.json`.

## Portal Setup Following

Once your `portal-versions.yml` file is ready, it is time to run the [portals-setup-following.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-following) script and don't forget to add the `portal-versions.yml` as a parameter.

**Example**

&#x20;`./scripts/portals-setup-following.sh -e @my-vars/portal-versions.yml --limit <server name>` executed from the `ansible-playbooks` directory.

{% hint style="info" %}
**NOTE** the first run of this script will timeout and fail. This is expected. Please see the below sections on options to handle completing this step.
{% endhint %}

#### MongoDB Replicaset Initialization

{% hint style="danger" %}
**NOTE:** There is currently a bug in the ansible playbook that affects the initial server for a portal and the mongo replicaset will not be initiated properly. Once fix this step will be removed from the documentation.
{% endhint %}

The ansible playbook will fail with the following error:

```
TASK [Fail on all wallet unlock errors except rescan in progress, which is handled later] ********************************************************************************************************************************************* 
fatal: [...]: FAILED! => { 
    "changed": false 
}

MSG:
Error unlocking Sia wallet: 
```

The mongo container will start up without error, but the replicaset will not be initialized. You can confirm this error by checking the mongo container logs for initialization errors.

```
docker logs mongo
```

To fix the issue, and initialize mongo, run the following commands:

```
# Navigate to webportal directory and get into the mongo container
cd ~/skynet-webportal
. .env && docker exec -it mongo mongo -u admin -p $SKYNET_DB_PASS

# Initiate the replicaset
# <serverdomain> should be the domain of the server. So mydomain.com for a single
# server, or sev1.mydomain.com for a multi-server portal.
rs.initiate(
  {
    _id : "skynet",
    members: [
      { _id : 0, host : "<serverdomain>:27017" },
    ]
  }
)
```

Now that mongo is properly initialized, re-run the `portals-setup-following.sh` ansible playbook.

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
