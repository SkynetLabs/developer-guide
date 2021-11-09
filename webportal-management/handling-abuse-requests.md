# Handling Abuse Requests

## Not Everything Should Be On Skynet

Skynet makes creating apps and sharing content on the decentralized Internet easy. We have lots of incredible users creating and making things accessible in new and fascinating ways.

Like every other sharing service, we unfortunately also have to deal with illegal content being uploaded to Skynet, and various organizations will alert portal operators to this fact.

We believe and recommend that taking care of these requests is important for two reasons:

* Most importantly, it helps ensure that individual or organization's rights aren't being violated.
* From a business perspective, your server hosts usually get looped into these requests, and can be brought down for hosting illegal content or other content that violates their terms of use. Read more about how we handle these requests here.

## Blocking Content

Skynet supports portals managing a blocklist. The blocklist is a list of content that the portal will block by not allowing it to be uploaded, downloaded, or pinned.

To block a skylink on a portal, you can use the [Ansible Block Skylink Playbook](https://github.com/SkynetLabs/ansible-playbooks#block-portal-skylinks). This is ideal of high priority requests, or if your portal does not receive a large number of abuse reports.

If you are running a large portal fleet, and receive a large number of abuse reports, we offer support for syncing an Airtable list. To enable Airtable, you will need to define the `AIRTABLE_API_KEY` in you `.env` file and update the Airtable base, table name, and field [here](https://github.com/SkynetLabs/skynet-webportal/blob/master/setup-scripts/blocklist-airtable.py#L16).

The portals have a cronjob that will sync the Airtable to the portal's blocklist every 4hrs.&#x20;
