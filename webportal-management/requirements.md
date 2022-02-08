---
description: Below are the prerequisites for setting up a Public Web Portal server.
---

# Prerequisites

## Overview

Here are the goals for each of the prerequisites.

### DNS

#### Goal

The goal for the DNS Prerequisite is that you have a domain name and you have the ability to manage the DNS records for the domain name. For multi-server portals, you will also need the ability to set up a load balancer for the portal.

#### Steps

1. Buy a domain name
2. Create an account where you can properly manage the DNS records and any required load balancing for the domain

### Hosting Provider

#### Goal

In order to run a Webportal, you will need 1 or more servers. At the end of this section, you should have a newly initialized server that meets the requirements and is accessible via `ssh` for the `root` user or compatible `sudo` user.&#x20;

#### Steps

1. Buy a server(s)
2. Confirm you have root ssh access

### Secrets Management

#### Goal

Set up your secrets management process for your portal such that ansible can be used.

#### Steps

1. Create a LastPass Account (Plain Text option coming soon)
2. Initialize config files

### Ansible

#### Goal

Set up ansible.

#### Steps

1. Follow the README instructions in the [ansible-playbooks](https://github.com/SkynetLabs/ansible-playbooks) repo.
   1. Docker installed
   2. Repo cloned
   3. [ansible-private](https://github.com/SkynetLabs/ansible-private-sample) repo cloned&#x20;

### SiaCoin

#### Goal

Running a portal requires SiaCoins (SC). If you already have experience acquiring SC, then you can skip this section.

If you are new to acquiring SC, check out the Sia Foundation's page on acquiring SC [https://sia.tech/get-siacoin](https://sia.tech/get-siacoin)

#### Steps

1. Buy SiaCoin&#x20;
