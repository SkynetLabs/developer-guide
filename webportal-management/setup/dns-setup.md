# DNS Setup

In this section, we will cover the basic DNS setup for a single server portal.

## Requirements

This section assumes that you have already purchased a domain for your portal and have created an AWS account.

## AWS

Currently, this documentation is set up to walk you through using AWS for your DNS management.

### Route53

#### Hosted Zones

Once you have created an account and logged in, head to Route53. You can find this under `Services` or by searching for it in the search bar.

In Route53, click on `Hosted Zones` and create a new hosted zone. This hosted zone should be public and is your domain that you purchased for your portal.

![](<../../.gitbook/assets/Hosted Zone Creation.png>)

#### Nameservers

Once your hosted zone is set up, AWS will have populated a nameserver `NS` record for your domain. Typically there are 4 nameservers listed in the `NS` record. You will need to update the nameservers for your domain where you purchased and managed your domain.&#x20;

If you happen to purchase your domain through NameCheap, you can update your nameservers by clicking on your domain, selecting the `Custom DNS` option under NameServers, and copying in the name servers that AWS generated for you.

![](<../../.gitbook/assets/Screen Shot 2022-01-05 at 2.55.33 PM.png>)

#### A Records

After you have created your hosted zone, you can now create additional DNS records.

First, you will want to create an A Record that points from your domain to your server's IP address.&#x20;

![](<../../.gitbook/assets/Route53 A record.png>)

Next, you will want to create a wildcard A record. To do this, you create an A record, select the `alias` option and find the A record that you just created that points to the server IP. For the value of `Record name` place an asterisk (\*).

![](<../../.gitbook/assets/Route53 Wildcard A record.png>)

## Verify

DNS records can some times take awhile to update. However, you will know that everything is set up properly when you can ssh into your server with the domain name instead of the IP address, like so:

```
ssh root@mydomain.com
```

