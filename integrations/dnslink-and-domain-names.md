# DNSLink & Domain Names

Skynet supports the [DNSLink](https://dnslink.dev/) protocol for serving skyfiles from behind a traditional domain name by setting special DNS records.

This means users can visit `yourDomain.com` and Siasky.net will serve your web application from Skynet with no traditional hosting company.

## Setting DNS records

Set your domain or subdomain's **CNAME** record to `siasky.net` and a **TXT** record with the name of `_dnslink` \(as a subdomain to the target domain name\) and its content field as `dnslink=/skynet-ns/[skylink]`

![Example DNS Records for CNAME and TXT records for siasky.tools](../.gitbook/assets/image%20%282%29.png)

## Dealing with SSL Certificates

At the moment, portals will not issue SSL Certificates for domains using DNSLink. The portal will use self-signed certificates, but you will need the DNS layer to take care of HTTPS support.

One option for this by using Cloudflare to manage your DNS records, then set the SSL/TLS encryption mode to Full.

![Cloudflare&apos;s SSL/TLS encryption mode set to Full](../.gitbook/assets/image%20%283%29.png)

## Other Limitations

At the moment, some aspects of the Skynet ecosystem are not supported when using DNSLink names. Uploads and downloads are not traced to a user with a portal account, so uploads directly to siasky.net from your site will only be pinned for 90 days.

