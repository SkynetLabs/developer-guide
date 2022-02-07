# AWS

{% hint style="info" %}
**NOTE:** Currently, this documentation is set up to walk you through using AWS for your DNS management.
{% endhint %}

{% hint style="info" %}
**NOTE:** You will get charges for your AWS usage. Even on our large portals, these charges have been minimal. Current rates as of Feb 2022, are $0.50 for a hosted zone and $0.40 per million queries. Most small portal operators will see less than $1 in AWS usage.&#x20;
{% endhint %}

### Account Creation

If you already have an AWS account you can skip this section, if not, continue on.&#x20;

Head to [aws.amazon.com](https://aws.amazon.com) and create an AWS account. You will want to create an account as a root user.

### Access Key

Now that you have your account created, you need to generate an access key.

Click on `Security Credentials` in your account dropdown menu.

Under `Access keys`, click `Create New Access Key`.

{% hint style="warning" %}
This next step should be updated to be an input variable in the ansible config file so that user's don't need to generate a common.yml file.
{% endhint %}

Save the information in the `common.yml` file you created in your `portal-common-configs` LastPass folder. The `AWSAccessKeyID` is the `aws_access_key` and the `AWSSecretKey` is the `aws_secret_access_key`.
