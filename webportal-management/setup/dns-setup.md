# DNS Setup

In this section, we will cover the basic DNS setup for a single server portal.

## Requirements

This section assumes that you have already purchased a domain for your portal.

## AWS

Currently, this documentation is set up to walk you through using AWS for your DNS management.

### Account Creation

If you already have an AWS account you can skip this section, if not, continue on.&#x20;

Head to [aws.amazon.com](https://aws.amazon.com) and create an AWS account. You will want to create an account as a root user.

### Route53

#### Hosted Zones

Once you have created an account and logged in, head to Route53. You can find this under `Services` or by searching for it in the search bar.

In Route53, click on `Hosted Zones` and create a new hosted zone. This hosted zone should be public and is your domain that you purchased for your portal.

![](<../../.gitbook/assets/Hosted Zone Creation.png>)

#### A Records

After you have created your hosted zone, you can now create additional DNS records.

First, you will want to create an A Record that points from your domain to your server's IP address.&#x20;

![](<../../.gitbook/assets/Route53 A record.png>)

Next, you will want to create a wildcard A record. To do this, you create an A record, select the `alias` option and find the A record that you just created that points to the server IP. For the value of `Record name` place an asterisk (\*).

![](<../../.gitbook/assets/Route53 Wildcard A record.png>)
