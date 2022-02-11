# DNS Setup

## Overview

### Goals

The goal of this section is to get your DNS setup for your portal. For single server portals, this just requires A records and for multi-server portals, this requires A records and a load balancer.

### Steps

#### Single Server Portal

1. Create Hosted Zone
2. Update name servers
3. Set A Records

#### Multi-Server Portal

1. Create Hosted Zone
2. Update name servers
3. Set A Records
4. Create API Keys
5. Setup Load Balancer

## AWS

Currently, this documentation is set up to walk you through using AWS for your DNS management.

{% hint style="info" %}
**NOTE:** Currently, this documentation is set up to walk you through using AWS for your DNS management.
{% endhint %}

### Route53

#### Hosted Zones

Once you have created an account and logged in, head to Route53. You can find this under `Services` or by searching for it in the search bar.

In Route53, click on `Hosted Zones` and create a new hosted zone. This hosted zone should be public and is the domain that you purchased for your portal.

![](<../../.gitbook/assets/Hosted Zone Creation.png>)

#### Nameservers

Once your hosted zone is set up, AWS will have populated a nameserver `NS` record for your domain. Typically there are 4 nameservers listed in the `NS` record. You will need to update the nameservers for your domain where you purchased and managed your domain.&#x20;

![](<../../.gitbook/assets/Screen Shot 2022-01-21 at 10.40.10 AM.png>)

If you happen to purchase your domain through NameCheap, you can update your nameservers by clicking on your domain, selecting the `Custom DNS` option under NameServers, and copying in the name servers that AWS generated for you.

![](<../../.gitbook/assets/Screen Shot 2022-01-05 at 2.55.33 PM.png>)

#### A Records

Follow the steps to set up your A records for your server, either as a [single server portal](dns-setup.md#undefined) or as a [multi-server portal](dns-setup.md#multi-server-portal-a-records).

#### Single Server A Records

After you have created your hosted zone, you can now create additional DNS records.

First, you will want to create an A Record that points from your domain to your server's IP address.&#x20;

![](<../../.gitbook/assets/Route53 A record.png>)

Next, you will want to create a wildcard A record. To do this, you create an A record, select the `alias` option and find the A record that you just created that points to the server IP. For the value of `Record name` place an asterisk (\*).

![](<../../.gitbook/assets/Route53 Wildcard A record.png>)

#### Multi-Server A Records

The process for setting A records for a multi-server portal is the same as for a single server portal, with the only change being the A record pointing to the server domain instead of the portal domain.

So if your portal domain is `mydomain.com` and you're setting up a server at `sev1.mydomain.com`, you would have an A record for `sev1.mydomain.com` pointing to the IP of the server and a wildcard A record for `*.sev1.mydomain.com` pointing to the first record `sev1.mydomain.com`.

#### CNAME Record

If you want your domain to be accessible at both `mydomain.com` and `www.mydomain.com` you will need to add a CNAME record for `www` that points to `mydomain.com` like so:

![](<../../.gitbook/assets/Screen Shot 2022-02-11 at 1.04.33 PM.png>)

## Verify

DNS records can sometimes take a while to update. However, you will know that everything is set up properly when you can ssh into your server with the domain name instead of the IP address, like so:

```
# Single Server Portal
ssh root@mydomain.com

# Multi-Server Portal
ssh root@sev1.mydomain.com
```

## Load Balancer

If you are running a multi-server portal, you will need to set up a load balancer to load balancer your domain across your servers.

### Access Key

The load balancer requires an access key, so you will need to generate an access key for your account.

Click on `Security Credentials` in your account dropdown menu.

Under `Access keys`, click `Create New Access Key`.

Save the information in the `common.yml` file you created in your `portal-common-configs` LastPass folder. The `AWSAccessKeyID` is the `aws_access_key` and the `AWSSecretKey` is the `aws_secret_access_key`.

### Load Balancer Setup

Coming soon...

