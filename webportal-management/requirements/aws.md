# DNS

## TL;DR

### Goal

The goal for the DNS Prerequisite is that you have a domain name and you have the ability to manage the DNS records for the domain name. For multi-server portals, you will also need the ability to set up a load balancer for the portal.

### Steps

1. Buy a domain name
2. Create an account where you can properly manage the DNS records and any required load balancing for the domain

## Domain Name

If you are setting up a public webportal, you will need your own domain name for your webportal. If you are just setting up a single portal then this is really all you need. &#x20;

One option for purchasing a domain name is through [NameCheap](https://namecheap.com). Search for a domain name you would like and follow the steps to purchase the domain name and step up your account.&#x20;

## AWS

{% hint style="info" %}
**NOTE:** Currently, this documentation is set up to walk you through using AWS for your DNS management.
{% endhint %}

{% hint style="info" %}
**NOTE:** You will get charges for your AWS usage. Even on our large portals, these charges have been minimal. Current rates as of Feb 2022, are $0.50 for a hosted zone and $0.40 per million queries. Most small portal operators will see less than $1 in AWS usage.&#x20;
{% endhint %}

### Account Creation

If you already have an AWS account you can skip this section, if not, continue on.&#x20;

Head to [aws.amazon.com](https://aws.amazon.com) and create an AWS account. You will want to create an account as a root user.

###
