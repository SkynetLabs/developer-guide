---
description: This assumes you have admin rights to a discord server to generate webhooks.
---

# Discord Notifications

The Skynet Webportals are set up to send notifications to discord to report various checks on the portals. Discord notifications are controlled by the following fields in the `.env` file:

```
# Used for Discord notifications integration.
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/...
DISCORD_MENTION_ROLE_ID=1234567890
```

### Webhook

To get the webhook for `DISCORD_WEBHOOK_URL` you will need to first go to the server settings of your discord server.

![](<../../.gitbook/assets/image (17).png>)

Then Click on Webhooks under Integrations.

![](<../../.gitbook/assets/Screen Shot 2022-02-04 at 4.22.38 PM.png>)

Click on `New Webhook` and give it a name and select the channel that it will post to. Then copy the webhook url with the `Copy Webhook URL` button and paste it in your `.env` file.

![](<../../.gitbook/assets/Screen Shot 2022-02-04 at 4.23.02 PM.png>)

You should start seeing messages populating in the channel.

![](<../../.gitbook/assets/Screen Shot 2022-02-04 at 4.23.39 PM.png>)

### Role Mention

If you want the messages to ping a role or user, such as `@devops`, you can use the `DISCORD_ROLE_MENTION_ID`.

Again, go to your server settings.

![](<../../.gitbook/assets/image (18).png>)

Click on members, find the user or role you would like to mention in the message. Click on the 3 dot menu and copy their role ID with `Copy ID` and paste it into your `.env` file.&#x20;

![](<../../.gitbook/assets/Screen Shot 2022-02-04 at 4.36.30 PM (1).png>)
