# AWS

Currently, this documentation is set up to walk you through using AWS for your DNS management.

### Account Creation

If you already have an AWS account you can skip this section, if not, continue on.&#x20;

Head to [aws.amazon.com](https://aws.amazon.com) and create an AWS account. You will want to create an account as a root user.

### Access Key

Now that you have your account created, you need to generate an access key.

Click on `Security Credentials` in your account dropdown menu.

Under `Access keys`, click `Create New Access Key`.

Save the information in the `common.yml` file you created in your `portal-common-config` LastPass folder. The `AWSAccessKeyID` is the `aws_access_key` and the `AWSSecretKey` is the `aws_secret_access_key`.
