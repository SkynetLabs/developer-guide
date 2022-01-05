# LastPass

Currently, a number of the ansible scripts use LastPass for managing server credentials and common files. If you plan to use the ansible scripts it is currently required to have a LastPass account.

Within LastPass, ansible is going to look for a folder where you are storing portal-related information. You can call this folder whatever you like, or you can use the default `Shared-Ansible`.

The top-level folder then will need 3 subfolders that will contain information solely managed by ansible. Again you can name these whatever you would like, or use the default names `portal-common-configs`, `portal-cluster-configs`, and `portal-server-configs.`

Your LastPass should look like the following:

```
Share-Ansible/
   portal-common-configs/
   portal-cluster-configs/
   portal-server-configs/
```

