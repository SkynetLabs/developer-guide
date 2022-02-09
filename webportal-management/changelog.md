# Changelog

## February 9th, 2022

The `common.yml` file is now optional. Here are some steps to make sure you have the updates

1. Update your `ansible-playbooks` repo with `git pull origin master`
2. Move your variables in your `common.yml` to your `cluster-{cluster-id}.yml` file
   1. We recommend keeping a backup of your `common.yml` file.
3. If using LastPass, update your LastPass folder variables if you are not using the defaults

See more information in the #portal-operators channel on the Skynet Labs discord [here](https://discord.com/channels/341359001797132308/674433179968602113/940968768715251712).
