# Hosting Provider

## Overview

At the end of this section, you should have a newly initialized server that meets the [requirements](../requirements.md) and is accessible via `ssh` for the `root` user or compatible `sudo` user.&#x20;

## Order a New Server

The first step is to order a new server from your hosting provider that meets the [requirements](../requirements.md) listed in the beginning of this section.&#x20;

Here is a list of some hosting providers to consider:

* [https://www.hetzner.com/](https://www.hetzner.com)
* [https://www.ovh.com/world/](https://www.ovh.com/world/)
* [https://skynode.eu/](https://skynode.eu)
* [https://dedicated.com/](https://dedicated.com)

## Install OS

Once the server has been purchased, the next step will be to install the [recommended OS](../requirements.md#system-os). Each hosting provider's UI is slightly different. Sometimes this is part of the server purchase and other times this must be done after the server is added to your account.&#x20;

## Verify SSH Access

Once the OS is installed, verify that you have ssh access to the `root` user, or a similar `sudo` user.

```
ssh root@<host>
```

If you are able to successfully ssh into the `root` user then you are done and can continue to [Server Initialization](server-initialization.md).

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

