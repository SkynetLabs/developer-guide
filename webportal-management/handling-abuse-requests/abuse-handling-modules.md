---
description: tools and setup for combating abusive content
---

# Abuse Handling

As with any cloud storage solution, there will be entities that upload and share abusive content through your web portal. It is important to handle abuse reports with the greatest care and priority and ensure the content being reported is prevented from being accessed as swiftly as possible.

There are three modules that help automate this process.

1. `blocker` module
2. `malware-scanner` module
3. `abuse-scanner` module

### Blocker Module

The `blocker` is a module that exposes a simple API that allows blocking a certain Skylink. Blocking a Skylink means that it will be marked as inaccessible for legal reasons by both Nginx and the `skyd` module. This module is arguably the most important one, both the abuse (email) scanner and the malware scanner talk to the `blocker` module directly.

#### Allowlist

The allow list is a list of Skylinks which can not be blocked. This is a feature that allows you to specify a list of Skylinks that you want to ensure don't get blocked by accident, or by someone that is purposefully trying to disrupt your service. This list is defined in a collection in the mongo database and should be manually edited. There is more information in the [project's README](https://github.com/SkynetLabs/blocker/blob/main/README.md) on how to add Skylinks to the allow list.

#### Environment

The blocker needs to be configured using the following set of environment variables. Essentially there are four things that the blocker needs to be configured for, which are: talk to `skyd`, talk to accounts, talk to Nginx, and connect to the mongo database.

* `API_HOST`: skyd host (defaults to `sia`)
* `API_PORT`: skyd port (defaults to `9980`)
* `NGINX_HOST`: Nginx host (defaults to `10.10.10.30`)
* `NGINX_PORT`: Nginx port (defaults to `8000`)
* `SIA_API_PASSWORD`: password of the skyd API
* `SKYNET_DB_HOST`: mongo database host
* `SKYNET_DB_PORT`: mongo database port
* `SKYNET_DB_USER`: mongo database user
* `SKYNET_DB_PASS`: mongo database password
* `SKYNET_ACCOUNTS_HOST`: accounts host (defaults to `accounts`)
* `SKYNET_ACCOUNTS_PORT`: accounts port (defaults to `3000`)
* `SERVER_DOMAIN`: the server domain
* `BLOCKER_LOG_LEVEL`: log level (defaults to `info`)

#### API

The `blocker` API has a simple health endpoint, alongside two types of 'block' endpoints. There is one externally facing endpoint which involves some proof of work to be done before the request is authorized.

The `/block` route is meant for internal use and should not be exposed. This endpoint does not require any proof of work and will check the reported Skylink against the allow list. If it is not on there, it will forward it to `skyd` (this is not entirely true, it actually will contact `skyd` through Nginx, to ensure the Nginx cache gets updated with the Skylink being blocked).

* **GET** `/health`: returns the status of the service
* **POST** `/block`: allows blocking a Skylink
* **POST** `/powblock`: allows blocking a Skylink, but requires some proof of work&#x20;
* **GET** `/powblock`: returns the target for the proof of work

### Malware Scanner Module

The `malware-scanner` is a module that uses [`ClamAV`](https://www.clamav.net)to scan Skylinks for malware. The scanner will scan all Skylinks that are added to the queue through the small API this module exposes. So essentially you can have other services enqueue Skylinks for scanning. If the scan comes back positive the Skylink will be blocked automatically through the use of the `blocker` module.&#x20;

#### Environment

The malware scanner needs to be configured using the following set of environment variables. Essentially there are three things that the malware scanner needs to be configured for, which are: talk to ClamAV, talk to the blocker, connect to the mongo database.

* `BLOCKER_IP`: IP address of the blocker container
* `BLOCKER_PORT`: port of the blocker container
* `CLAMAV_IP`: IP address of the ClamAV container
* `CLAMAV_PORT`: port of the ClamAV container
* `SKYNET_DB_HOST`: mongo database host
* `SKYNET_DB_PORT`: mongo database port
* `SKYNET_DB_USER`: mongo database user
* `SKYNET_DB_PASS`: mongo database password
* `PORTAL_DOMAIN`: the Skynet portal domain
* `SERVER_DOMAIN`: the server domain
* `MALWARE_SCANNER_LOG_LEVEL`: log level (defaults to `info`)

#### API

The `malware-scanner` API has a simple health endpoint, alongside an endpoint that allows scheduling a Skylink for scanning.

* **GET** `/health`: returns the status of the service
* **POST** `/scan/:skylink`: allows adding a Skylink to the queue to get scanned

### Abuse Scanner Module

The `abuse-scanner` module is a module that you can configure to scan a mailbox. It will periodically fetch new emails, scan them for Skylinks and a certain set of abuse tags, and then try and block the Skylinks it can find. After it has blocked the abusive content, it will reply to the email in question with an automatically generated report to list the Skylinks it blocked and/or whether it was successful in doing so.

#### Setup

The current implementation of the module will automatically block all Skylinks it can find in the email. Regardless of whether it can find abuse tags like "phishing" or "copyright" in the email. That is why the scanner is targeted at a mailbox, instead of simply defaulting to the inbox. Seeing as the Skylinks are blocked immediately, it might be advisable to point to scanner to a mailbox to which mails are forwarded based on a certain rule set. For instance, a label could be applied when the subject contains a certain word, or if the sender equals a certain email address. This is of course all configurable, there is nothing preventing you from pointing it towards the Inbox.

#### Environment

The abuse scanner needs to be configured using the following set of environment variables. Essentially there are three things that the abuse scanner needs to be configured for, which are: talk to the IMAP server, talk to the blocker, connect to the mongo database.

* `BLOCKER_HOST`: host of the blocker container (ex: `blocker`)
* `BLOCKER_PORT`: port of the blocker container (ex: `4000`)
* `EMAIL_SERVER`: IMAP server URL (ex: `imap.gmail.com:993`)
* `EMAIL_USERNAME`: email account username (ex: `devs@siasky.net`)
* `EMAIL_PASSWORD`: email account password (ex: `supersecret`)
* `SKYNET_DB_HOST`: mongo database host
* `SKYNET_DB_PORT`: mongo database port
* `SKYNET_DB_USER`: mongo database user
* `SKYNET_DB_PASS`: mongo database password
* `SERVER_DOMAIN`: the server domain
* `ABUSE_MAILBOX`: the name of the inbox to scan (ex: `INBOX`)
* `ABUSE_SPONSOR`: the name of the sponsor (used as the `reporter` field for the `blocker`)
* `ABUSE_MAILADDRESS`: the email address to send the abuse report to (ex: `devs@siasky.net`)
* `ABUSE_LOG_LEVEL`: log level (ex: `debug`)

#### Architecture

The scanner is comprised out of 4 sub-modules:

1. fetcher
2. parser
3. blocker
4. finalizer

This is done in order for it to be easy to swap out a module for a service that is custom-written and works off of the same database. If you for instance want to reimplement the `parser` or adjust the `blocker` you can easily do so, and even use the language of your choosing, as long as you work off of the same database and apply more or less the same pattern.



For more information on the architecture of the abuse scanner, please refer to its [README](https://github.com/SkynetLabs/abuse-scanner/blob/main/README.md) file.
