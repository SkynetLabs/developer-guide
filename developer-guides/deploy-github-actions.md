---
description: >-
  notes: first link should be to marketplace? update karol article? Do we do a
  walkthrough here?
---

# Deploy-to-Skynet Github Action

## Introduction

After working with Skynet deployments for some time, you'll want to automate your deploy pipeline. Skynet Labs has Github Action you can easily incorporate into your workflow to automate uploading builds to Skynet.

When added as a step in a workflow, the [Deploy to Skynet action](https://github.com/SkynetLabs/deploy-to-skynet-action) will upload a specified directory to Skynet, and, if given a seed, will also create a resolver skylink that points to the upload and will not change after subsequent deploys.

The guide below walks through the steps, but for a full walkthrough, be sure to see Karol's article on [Automated deployments on Skynet](https://blog.sia.tech/automated-deployments-on-skynet-28d2f32f6ca1).

## Adding the Action to a Workflow

If you're already familiar with Github Actions, add an action to a workflow by adding a step like the following:

```yaml
      - name: Deploy to Skynet
        uses: SkynetLabs/deploy-to-skynet-action@v2
        with:
          upload-dir: public
          github-token: ${{ secrets.GITHUB_TOKEN }}
          registry-seed: ${{ secrets.REGISTRY_SEED || '' }}
          registry-datakey: ${{ secrets.REGISTRY_DATAKEY || '' }}
```

### Inputs

#### `upload-dir`

**Required** Directory to upload \(usually `build`, `dist`, `out` or `public`\).

This action requires the upload directory to be already available so you will need to run the build step before running this action.

#### `github-token`

**Required** Your github token that is required to authenticate posting a comment on pull request.

Find out more about github token from [documentation](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow).

#### `registry-seed`

You can provide a seed \(keep it secret, keep it safe\) and this action will set corresponding skynet registry entry value to the deployed resolver skylink.

Public link to the registry entry will be printed in the action log.

#### `registry-datakey`

Default value: `skylink.txt`

You can define custom datakey for a registry entry when used with `registry-seed`. Change only if you want to use a specific key, default value will work in all other cases.

#### `portal-url`

Default value: `https://siasky.net`

You can override default skynet portal url with any compatible community portal or self hosted one.

### Outputs

#### `skylink`

The resulting skylink.

Example: `sia://IAC6CkhNYuWZqMVr1gob1B6tPg4MrBGRzTaDvAIAeu9A9w`

#### `skylink-url`

The resulting skylink url \(base32 encoded skylink in subdomain\).

Example: `https://400bk2i89lheb6d8olltc2grqgfaqfge1im134ed6q1ro0g0fbnk1to.siasky.net`

#### `resolver-skylink`

A resolver skylink pointing at the resulting skylink. Resolver skylink will remain the same throughout the deploys, but will always resolve to the latest deploy.

Example: `sia://AQDwh1jnoZas9LaLHC_D4-2yP9XYDdZzNtz62H4Dww1jDA`

#### `resolver-skylink-url`

The resulting resolver skylink url \(base32 encoded skylink in subdomain\). Resolver skylink will remain the same throughout the deploys, but will always resolve to the latest deploy.

Example: `https://040f11qosugpdb7kmq5hobu3sfmr4fulr06tcspmrjtdgvg3oc6m630.siasky.net/`

For more info, see the [Deploy to Skynet action repo](https://github.com/SkynetLabs/deploy-to-skynet-action).

{% hint style="info" %}
Be sure to use a specific version of the action. Version 2 introduced resolver skylinks, replacing the `skyns` usage from earlier versions.
{% endhint %}

{% hint style="warning" %}
The Github Action makes use of saving Encrypted Secrets on Github's servers. Avoid re-using seed phrases across contexts or repositories.
{% endhint %}

## Example deploy-to-skynet.yml

This file is a good place to start. In your project, create a  `.github/workflows/deploy-to-skynet.yml`file.

```yaml
name: Deploy to Skynet

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x

      - run: yarn
      - run: yarn build

      - name: "Deploy to Skynet"
        uses: SkynetLabs/deploy-to-skynet-action@v2
        with:
          upload-dir: build
          github-token: ${{ secrets.GITHUB_TOKEN }}
          registry-seed: ${{ secrets.SKYNET_REGISTRY_SEED || '' }}
```

## Futher Reading

{% embed url="https://blog.sia.tech/automated-deployments-on-skynet-28d2f32f6ca1" caption="" %}

{% embed url="https://docs.github.com/en/actions/quickstart" caption="" %}

{% embed url="https://docs.github.com/en/actions/reference/encrypted-secrets" caption="" %}

{% embed url="https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions" caption="" %}

