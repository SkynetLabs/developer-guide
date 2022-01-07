# LastPass

## Folder Structure

Currently, a number of the ansible scripts use LastPass for managing server credentials and common files. If you plan to use the ansible scripts it is currently required to have a LastPass account.

Within LastPass, ansible is going to look for a folder where you are storing portal-related information. You can call this folder whatever you like, or you can use the default `Shared-Ansible`.

{% hint style="warning" %}
This folder needs to be a `Shared` folder in LastPass for ansible to be able to properly interact with it. When you create a shared folder in LastPass it will automatically add the `Shared` prefix.
{% endhint %}

The top-level folder then will need 3 subfolders that will contain information managed by ansible. Again you can name these whatever you would like, or use the default names `portal-common-configs`, `portal-cluster-configs`, and `portal-server-configs.`

Your LastPass should look like the following:

```
Share-Ansible/
   portal-common-configs/
   portal-cluster-configs/
   portal-server-configs/
```

## Files

{% hint style="warning" %}
This Files section is currently a manual process that needs to be moved to ansible.
{% endhint %}

### portal-common-configs

Under the `portal-common-configs` subfolder, create a `common.yml` file and put the following fields in the file.

```
aws_access_key: 
aws_secret_access_key:
discord_bot_token:
serverlist_entropy: 
serverlist_tweak: 
airtable_api_key:
airtable_base:
airtable_table: 
airtable_field:
s3_backup_path:
```

You can leave these fields blank for now, and we will come back to them later.&#x20;

### portal-cluster-configs

Under the `portal-cluster-configs` subfolder, create a `cluster-prod.yml` file and put the following fields in the file.

```
accounts_email_uri: 
cookie_enc_key:
cookie_hash_key:
mongo_db_mgkey: |
portal_cluster_domain: 
skynet_db_user: 
skynet_db_pass: 
skynet_db_replicaset: 
stripe_api_key: 
stripe_publishable_key: 
stripe_secret_key: 
stripe_webhook_secret: 
```

You can leave these fields blank for now, with the exception of the `|` for the `mongo_db_mgkey` field, and we will come back to them later.&#x20;
