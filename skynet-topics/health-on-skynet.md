# Health on Skynet

## Introduction

There are a number of ways of assessing the "health" of piece of the Skynet network. Portals provide API endpoints that provide insight into the health of a skyfile, registry entry, or a specific server in a portal.

## API Endpoints

{% hint style="warning" %}
Health endpoints are not fast and should probably be reserved for developer tools unless used with short timeouts or `nocache` set to false.
{% endhint %}

{% swagger baseUrl="https://siasky.net" path="/skynet/health/skylink/:skylink" method="get" summary="Skylink Health" %}
{% swagger-description %}
The health of a skylink is determined by how many servers are hosting full versions of the skyfile's base sector. By default, a new upload should have a value of 10. This method queries all hosts when determining how many copies of the base sector are stored across the network.

\




\


Portal will wait the entire duration of the timeout for hosts to respond, so longer timeouts mean higher accuracy.
{% endswagger-description %}

{% swagger-parameter in="path" name="skylink" type="string" %}
Base64 Skylink
{% endswagger-parameter %}

{% swagger-parameter in="query" name="timeout" type="number" %}
Timeout in seconds. Higher is more accurate. (Default: 30, Max: 300)
{% endswagger-parameter %}

{% swagger-response status="200" description="Base sector redundancy shows how many hosts contain full versions of the file's base sector.

fanouteffectiveredundancy — the redundancy of the fanout, represented as the worst redundancy of any of the chunks
fanoutdatapieces — the number of data pieces defined by the erasure coding of the fanout. Default is 10 for the 10-of-30 erasure coding that results in 3x redundancy
fanoutparitypieces — the number of parity pieces as defined by the erasure coding of the fanout. Default is 20 for the 10-of-30 erasure coding that results in 3x redundancy.
fanoutredundancy — an array of all chunk redundancies. So you can see if there is a single chunk that is bringing down the redundancy or if they are all a similar redundancy." %}
```javascript
{
    "basesectorredundancy": 18,
    "fanouteffectiveredundancy": 2.6,
    "fanoutdatapieces": 10,
    "fanoutparitypieces": 20,
    "fanoutredundancy": [
        2.6
    ]
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://siasky.net" path="/skynet/health/entry" method="get" summary="Registry Entry Health by publickey & datakey" %}
{% swagger-description %}
The health of a registry entry is determined by the number of hosts storing the most current version of the registry entry.

\




\


This method queries all hosts to determine:

\


\- 

`numbestentries`

 – Number of hosts on network with the "best" entry

\


\- 

`numbestprimaryentries`

 – Number of best primary entries on network 

_(Not yet in use.)_

\


\- 

`numentries`

 – Total number of entries found across the network

\


\- 

`revisionnumber`

– Current revision number of a "best" entry (highest on network)

\




\


Portal will wait the entire duration of the timeout for hosts to respond, so longer timeouts mean higher accuracy.
{% endswagger-description %}

{% swagger-parameter in="path" name="publickey" type="string" %}
Public key of registry entry. Should be prefixed with 

`ed25519:`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="datakey" type="string" %}
Data key of registry entry (Not pre-hashed as used by 

`skynet-js`

)
{% endswagger-parameter %}

{% swagger-parameter in="path" name="timeout" type="number" %}
Timeout in seconds. Higher is more accurate.

\


(Default: 30, Max: 300)
{% endswagger-parameter %}

{% swagger-response status="200" description="Define each of these." %}
```javascript
{
    "numbestentries": 29,
    "numbestprimaryentries": 0,
    "numentries": 63,
    "revisionnumber": 19
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://siasky.net" path="/skynet/health/entry" method="get" summary="Registry Entry Health by entryid" %}
{% swagger-description %}
Another way of accessing the health of a registry entry, using an entry's 

`entryid`

. See above for response details. 

_(`entryid` is a hash of the of the publickey and datakey used for looking up registry entries and is used for creating resolver skylinks.)_
{% endswagger-description %}

{% swagger-parameter in="path" name="entryid" type="string" %}
entryid of the registry entry 

_(See description.)_
{% endswagger-parameter %}

{% swagger-parameter in="path" name="timeout" type="number" %}
Timeout in seconds. Higher is more accurate. (Default: 30, Max: 300)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
    "numbestentries": 29,
    "numbestprimaryentries": 0,
    "numentries": 63,
    "revisionnumber": 19
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://siasky.net" path="/skynet/stats" method="get" summary="Portal Server Stats" %}
{% swagger-description %}
The health of a server in a portal can be accessed by its performance and key settings in its Sia node.
{% endswagger-description %}

{% swagger-parameter in="path" name="nocache" type="boolean" %}
Bypass cached results. (Default: false)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
    "basesectorupload15mdatapoints": 95.25281700939354,
    "basesectorupload15mp99ms": 11520,
    "basesectorupload15mp999ms": 12544,
    "basesectorupload15mp9999ms": 12544,
    "basesectoroverdrivepct": 0.6285375611201659,
    "basesectoroverdriveavg": 1.031708401244629,
    "fanoutsectoroverdrivepct": 0.9992122439792933,
    "fanoutsectoroverdriveavg": 2.814009195845793,
    "chunkupload15mdatapoints": 298.06404821577934,
    "chunkupload15mp99ms": 11520,
    "chunkupload15mp999ms": 12544,
    "chunkupload15mp9999ms": 15104,
    "registryread15mdatapoints": 466.73241750297063,
    "registryread15mp99ms": 92,
    "registryread15mp999ms": 416,
    "registryread15mp9999ms": 13824,
    "registrywrite15mdatapoints": 82.95849806578686,
    "registrywrite15mp99ms": 88,
    "registrywrite15mp999ms": 96,
    "registrywrite15mp9999ms": 112,
    "streambufferread15mdatapoints": 15359.115856056023,
    "streambufferread15mp99ms": 4352,
    "streambufferread15mp999ms": 6912,
    "streambufferread15mp9999ms": 11008,
    "systemhealthscandurationhours": 1.9730922626155556,
    "allowancestatus": "healthy",
    "contractstorage": 46330009354240,
    "maxstorageprice": "69444444444",
    "numcritalerts": 0,
    "numfiles": 381546,
    "portalmode": true,
    "repair": 122540785664,
    "storage": 2906511304081,
    "stuckchunks": 3324,
    "walletstatus": "low",
    "uptime": 12884,
    "versioninfo": {
        "version": "1.6.0-master",
        "gitrevision": "✗-30eb347fe"
    }
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://siasky.net" path="/health-check" method="get" summary="Portal Server Health Check" %}
{% swagger-description %}
Gets the results of a variety of internal checks on a portal server to see if the basic behavior is working correctly and if the server is "disabled."
{% endswagger-description %}

{% swagger-parameter in="path" name="nocache" type="boolean" %}
Bypass cached results. (Default: false)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
    "disabled": false,
    "entry": {
        "date": "2021-08-24T22:20:00.301Z",
        "checks": [
            {
                "name": "upload_file",
                "time": 4011,
                "up": true,
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "website",
                "time": 132,
                "up": true,
                "url": "https://siasky.net",
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "skylink",
                "time": 208,
                "up": true,
                "url": "https://siasky.net/AACogzrAimYPG42tDOKhS3lXZD8YvlF8Q8R17afe95iV2Q",
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "skylink_via_subdomain",
                "time": 205,
                "up": true,
                "url": "https://000ah0pqo256c3orhmmgpol19dslep1v32v52v23ohqur9uuuuc9bm8.siasky.net/",
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "hns_via_subdomain",
                "time": 1750,
                "up": true,
                "url": "https://note-to-self.hns.siasky.net/",
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "registry_write_and_read",
                "up": true,
                "time": 770
            },
            {
                "name": "server_api_access",
                "time": 77,
                "up": true,
                "url": "https://us-va-1.siasky.net",
                "statusCode": 200,
                "ip": "51.81.93.50"
            },
            {
                "name": "accounts",
                "time": 152,
                "up": true,
                "statusCode": 200,
                "response": {
                    "dbAlive": true
                },
                "ip": "51.81.93.50"
            },
            {
                "name": "account_website",
                "time": 841,
                "up": true,
                "url": "https://account.siasky.net/auth/login",
                "statusCode": 200,
                "ip": "51.81.93.50"
            }
        ]
    }
}
```
{% endswagger-response %}
{% endswagger %}
