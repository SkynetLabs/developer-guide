# MySky Files

## Introduction

One of the wonderful aspects of MySky is it allows users to control data linked to their MySky account. Applications can create "MySky Files" that can be then shared with other applications or users. The goal is to create a system where users have ownership of their files and data online, and can always move their existing data over to a new application that better suits their needs.

MySky Files are saved to a location that mimics a file system in an operating system. Every MySky file has a "path" and a "type", and applications are permissioned to root-level directory names.

## MySky File Paths

MySky Files use pathnames to distinguish between them. An application might save a user's preferences to `applicationName/preferences.json`. The first part of the path (before the first "/") is called the `dataDomain` and an application request permissions to a dataDomain.

{% hint style="info" %}
MySky Files are distinguished by the path _and_ type. A Discoverable File and an Encrypted File can share a "path" without over-writing one another.
{% endhint %}

## MySky File Types

### Discoverable Files

Discoverable Files are the most public MySky File type. They are associated with your MySky UserID, and often used for writing MySky Files to predictable path locations (or using indexes at predictable path locations) so other applications can easily read and "discover" the data.

Profile data and blog posts are great examples of data users want to be unencrypted and publicly accessible.

Discoverable Files use `mySky.getJSON` and `mySky.setJSON` methods, which can be read about in the [skynet-js docs](https://siasky.net/docs/?javascript--browser#getting-discoverable-json). Additionally, discoverable files for any UserID can be read using `client.file.getJSON( userId, path)`without permissions or loading a MySky client.

### Encrypted Files

Encrypted Files act much like Discoverable Files but use MySky to encrypt the data saved to Skynet. A user's UserID is still connected to the file, and the path and a secret user seed are needed to derive where the file is stored on Skynet and decrypt it.

Data that should not be publicly accessible, like shared notes between friends, is a good option for Encrypted Files.

Encrypted Files use `mySky.getJSONEncrypted` and `mySky.setJSONEncrypted` methods, which can be read about in the [skynet-js docs](https://siasky.net/docs/?javascript--browser#getting-encrypted-json).

### Hidden Files

Hidden Files prioritize privacy and security. They are currently unreleased but contain a number of features to guard files from being viewed by an attacker, linked to the user, or linked to other files uploaded at the same time. These files and directories are still able to be shared with friends. For more information, see the spec document for [Hidden Files on MySky](https://hackmd.io/Vs0IOgJyQPaePULFjODPGQ?view).

{% hint style="warning" %}
Hidden Files are not yet implemented in MySky, but are on the road map.
{% endhint %}

### Comparison

|                                     | Discoverable Files | Encrypted Files | Hidden Files |
| ----------------------------------- | :----------------: | :-------------: | :----------: |
| Encrypted                           |          ❌         |        ✅        |       ✅      |
| Associated with MySky User ID       |          ✅         |        ✅        |       ❌      |
| Accessible if UserID and Path Known |          ✅         |        ❌        |       ❌      |
| Permissions needed for Read access  |          ❌         |        ✅        |       ✅      |

## MySky Permissions & dataDomains

When you set `dataDomain`, you're requesting full permissions for skyfiles using that "data domain". A MySky file's dataDomain is the root-level directory of its path.

> The MySky file saved at `skyposts.hns/posts/123.json` has a dataDomain of `skyposts.hns`. To write to this file, you'd need permissions to the `skyposts.hns` dataDomain.

If your site's hosted url matches the dataDomain, the user is not prompted for permissions to access the dataDomain. If your site's url does not match the requested dataDomain, users will be asked to approve all permissions for accessing the requested dataDomain.

For example, a skapp accessed at `skyapp.hns.siasky.net` requesting access to the dataDomain of `skyapp.hns` will be automatically authorized and users will not be prompted for permissions. If that site is accessed using a skylink url like `30039k6ev3f6u2ftr1tei5vkn98ld7hjfbotctm9la1ofd1kv2rdlog.siasky.net`

and requests access to the `skyapp.hns` dataDomain, the user will be prompted for permissions.

### Requesting Additional Permissions

Your site can request permissions to other dataDomains using the `mySky.addPermissions` method. See the skynet-js documentation for [Adding Custom Permissions](https://siasky.net/docs/#adding-custom-permissions) for more info.

{% hint style="info" %}
Encrypted Files use Hidden File permissions. An app needs permissions to read or write to Hidden Files to access Encrypted Files.
{% endhint %}

{% hint style="info" %}
When adding permissions, you won't know your current domain beforehand, since your site might be run directly from its skylink. One way to get around this is to use the value from\
`await client.extractDomain(window.location.href)`
{% endhint %}

## Further Reading

{% embed url="https://hackmd.io/r-q0LhnLQF23Vk8bofZGKg" %}
Additional Information on Encrypted Files
{% endembed %}

{% embed url="https://hackmd.io/Vs0IOgJyQPaePULFjODPGQ" %}
Additional Information on Hidden Files
{% endembed %}

