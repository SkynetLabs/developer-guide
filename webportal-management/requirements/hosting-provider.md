# Hosting Provider

## Overview

In order to run a Webportal, you will need 1 or more servers. At the end of this section, you should have a newly initialized server that meets the requirements and is accessible via `ssh` for the `root` user or compatible `sudo` user.&#x20;

## Order a New Server

The first step is to order a new server from your hosting provider that meets the following recommended requirements.&#x20;

Here is a list of some hosting providers to consider:

* [https://www.ovh.com/world/](https://www.ovh.com/world/)
* [https://dedicated.com/](https://dedicated.com)

### Network

1 Gbps up and down is recommended.&#x20;

High limit or unlimited bandwidth is encouraged. Bandwidth usage is of course highly dependent on the usage of each portal but it is important to note, that even with low numbers of new uploads, repair bandwidth will continue to increase over time.&#x20;

### Hardware

The following are the recommendations for hardware requirements.&#x20;

{% hint style="info" %}
**NOTE:** the requirements are listed in order of importance. SSD is the most important, RAM is second, and CPU is last. Our experience has been that, unless you are building a custom server, by the time you have selected your SSD requirements and RAM requirements, most hosting providers only have a few or even just one option for CPU so do the best you can but don't worry too much about how close you are to matching the recommendation on CPU.
{% endhint %}

#### Storage

500GB SSD/NVMe or more. SSD is required, disk space will be most impacted by the amount of logging the server does.&#x20;

#### RAM

32GB of RAM or more.

#### CPU

Intel Xeon E-2136 or AMD Ryzen 5 3600 CPU or better.



## Install OS

Once the server has been purchased, the next step will be to install the recommended OS which is Debian 10. Each hosting provider's UI is slightly different. Sometimes this is part of the server purchase and other times this must be done after the server is added to your account.&#x20;

## Verify SSH Access

Once the OS is installed, verify that you have ssh access to the `root` user, or a similar `sudo` user.

```
ssh root@<host>
```

If you are able to successfully ssh into the `root` user then you are done and can continue to [Server Initialization](../setup/server-initialization.md).

If you either don't have a `root` user or are prompted for a password when `ssh`ing into the server, please see the following sections for help.

### Adding an SSH Key

Most hosting providers have a way to add ssh keys to your account so that they are added automatically when the OS is installed on new servers. Check your hosting provider's support documentation to see if this is an option.

If your hosting provider doesn't have an option for adding an ssh key during the OS install, you can add your ssh key manually.&#x20;

{% hint style="info" %}
**NOTE** you need the password for the user (i.e. `root`) in order to do this. If you don't have the password, and cannot ssh into the server with the password you will most likely need to re-install the OS to generate a new password with your hosting provider.&#x20;
{% endhint %}

To add your key manually, you can execute the following command:

```
ssh-copy-id -i ~/.ssh/mykey root@<host>
```

{% hint style="info" %}
See [https://www.ssh.com/academy/ssh/copy-id](https://www.ssh.com/academy/ssh/copy-id) for more information on copying over your ssh key
{% endhint %}

### Verifying Sudo User Permissions

Some new OS distributions will not create a `root` user for security purposes. In this case you might have a `debian` user for example. To verify that the user has the right `sudo` permissions you can use the following command after you have ssh'd into the server:

```
sudo -l -U <username>
```

This will print out the following:

```
Matching Defaults entries for <username> on <host>: 
   ...
   
User <username> may run the following commands on <host>:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
    (ALL) NOPASSWD: ALL
    (root) NOPASSWD: ALL
```

The key things here are `(ALL : ALL) ALL` and `NOPASSWD` which confirm that this user has root access and doesn't require a password with `sudo`.

{% hint style="info" %}
More info [here](https://unix.stackexchange.com/questions/50785/how-do-i-find-out-if-i-am-sudoer).
{% endhint %}

