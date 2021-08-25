# Server Initialization

## Overview

At the end of this section, you should have the DNS records added for the new server, the server initialized with a user `user`, and the `root` \(or other `sudo`\) user removed.

## DNS

Each server needs two A records. One A record that points from `<server name>.domain` to its IP address, and one wildcard A record that points from `*.<server name>.domain` to `<server name>.domain`.

**Example**:

| Record | Type | Value |
| :--- | :--- | :--- |
| us-west.siasky.net | A | XX.XX.XX.XX |
| \*.us-west.siasky.net | A | us-west.siasky.net |

## Prerequisites 

### LastPass

As mentioned in [system requirements](../system-requirements.md#applications), many ansible scripts use LastPass for managing server credentials and common files. If you haven't created a LastPass account yet you will need to create one now.

In your LastPass account, you will then need to create an entry for the server as seen below. Updating `<server name>` with the name of your server, i.e. `us-west`, and `domain` with your domain, i.e. `siasky.net`. Ansible will pull this information from LastPass in order to create the user `user` on the server with the provided password.

![](../../.gitbook/assets/screen-shot-2021-08-25-at-4.39.55-pm.png)

### Ansible

Prepare your local machine for running ansible by following the setup requirements [here](https://github.com/SkynetLabs/ansible-playbooks#requirements).

## Portal Setup Initial

To set up the portal, you will need to run the [portal-setup-initial.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-initial) ansible script. At this point you should have all the prerequisites complete except for:

* Logging in to LastPass with the[ lastpass-login.sh](https://github.com/SkynetLabs/ansible-playbooks#lastpass-login) script
* Adding your server to your `host.ini` file

After successfully running the [portal-setup-initial.sh](https://github.com/SkynetLabs/ansible-playbooks#playbook-portals-setup-initial) script you are ready to continue on to the [Single Portal Setup](single-portal-setup.md).

