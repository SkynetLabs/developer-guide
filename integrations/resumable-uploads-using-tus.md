# Resumable Uploads Using tus

In addition to basic HTTP POST uploads, Skynet supports uploading files using the [tus protocol](https://tus.io/). This enables uploading files up to 100GB for users with a portal account.

{% hint style="warning" %}
Only single file uploads are supported using tus. Directory uploads are not supported.
{% endhint %}

## Large file uploads using skynet-js

Large file uploads are automatically supported in `skynet-js` and `skynet-nodejs`. Any file over 40MB will automatically use the built-in tus upload client.

## Using tus Outside of skynet-js

Because this is a commonly used protocol, it [many integrations](https://tus.io/implementations.html) across many different environments and languages. If you want to use a different client, [following the code](https://github.com/SkynetLabs/skynet-js/blob/3f5ed945c2c64ab8abb7308aaea38656e0e2ede4/src/upload.ts#L199) in `skynet-js` may help, but here are some tips.

#### Upload Endpoint

The basic upload endpoint is `/skynet/tus` \(i.e. https://siasky.net/skynet/tus\)

#### Upload Flow

Upon making a POST request to the above endpoint, the response will contain a `location` header where PATCH requests are made. It should address a specific folder and have an upload identifier like this:`https://us-or-1.siasky.net/skynet/tus/1994a347bb0bafd0f92bc4620adb037b9e7b7aa6`

#### On Success

When the Upload is complete, you need to make a **HEAD** request to the `location` address mentioned above. The response should have a `skynet-skylink` header containing the upload's skylink.

#### Chunk Size

We recommend using a chunk size matching `const TUS_CHUNK_SIZE = (1 << 22) * 10`

#### Required Header

All requests should have the `Tus-Resumable` header set to the value of `1.0.0`

#### Cookies /JWT Token

To allow large file uploads over 1GB, your requests need a `skynet-jwt` cookie set and passed along as credentials with each request to the portal. See Using Your JWT in [Server-Hosted Skynet Usage](../developer-guides/server-hosted-skynet-usage.md#using-your-jwt).

## Full Description

> Chris has written a full desciption of the tus protocol on Skynet, reproduced below.

Unstable connections are a major concern when uploading files to a server. Especially when uploading large files over HTTP since it does not have any support for resuming an upload if it failed halfway through. You might be uploading a 50GB recording to share with your friends, to have your connection drop for a second after 49GB, and being forced to do it all over again. Many companies offering data storage have their own solution to this problem built into their APIs, SDKs, or sync clients. For Skynet, we have implemented the [TUS protocol](https://tus.io/). An open protocol for resumable uploads.

### TUS

TUS is an open, minimalistic, and extensible [protocol](https://tus.io/protocols/resumable-upload.html) that any client or server can implement. It splits features into so-called extensions. Apart from the core protocol, clients and servers can implement any number of extensions but are encouraged to implement as many as possible.

Each upload has a unique ID used by the core protocol to get information about the upload from the server in case the upload was interrupted. The following example from the official [documentation](https://tus.io/protocols/resumable-upload.html#core-protocol) demonstrates how the core protocol resumes uploads. The example assumes that a 100-byte upload was created and its ID obtained using the _Creation_ extension but was interrupted after 70 bytes.

The first request after the interruption is a HEAD request using the upload’s id. It also contains the version of the protocol used by the client.

```http
HEAD /files/24e533e02ec3bc40c387f1a0e460e216 HTTP/1.1
Host: tus.example.org
Tus-Resumable: 1.0.0
```

The server responds with the last known offset of the upload and its version.

```http
HTTP/1.1 200 OK
Upload-Offset: 70
Tus-Resumable: 1.0.0
```

Afterward, the client uses a PATCH request to send the remaining 30 bytes starting at offset 70. If the offset doesn’t match the server’s expectation, it will return an error.

```http
PATCH /files/24e533e02ec3bc40c387f1a0e460e216 HTTP/1.1
Host: tus.example.org
Content-Type: application/offset+octet-stream
Content-Length: 30
Upload-Offset: 70
Tus-Resumable: 1.0.0[remaining 30 bytes]
```

Upon success, the server responds with the new offset and a status code 204.

```http
HTTP/1.1 204 No Content
Tus-Resumable: 1.0.0
Upload-Offset: 100
```

Usually, a successful upload will consist of multiple sequential PATCH calls until the upload is finished. The amount of data uploaded with each call is called the chunk size. The larger the chunk size, the fewer PATCH calls are needed to finish the upload, but as they get larger, the chance that a chunk gets interrupted and needs to be reuploaded increases as well. Different services may place additional restrictions on the chunk size or the maximum allowed file size. For example, an object storage provider might enforce a minimum chunk size.

### Extensions

As mentioned above, a server can implement a set of extensions to add features. Not every extension may be suitable for every service. The following extensions exist at the time of writing.

* _Creation_: Creates a new upload
* _Creation with Upload_: Creates a new upload with initial payload
* _Expiration_: Set expiration for unfinished uploads
* _Checksum_: Consistency check for PATCH requests
* _Termination_: Client-side termination of completed/unfinished uploads
* _Concatenation_: Combination of uploads to enable parallel uploading

Right now, Skynet implements the _Creation_ and _Creation with Upload_ extensions without deferrable upload sizes. While it doesn’t currently support the full _Expiration_ extension, it will prune failed uploads right away and uploads after 20 minutes.

### Adaptions for Skynet <a id="0d61"></a>

Since Skynet uses a special way of uploading and requesting files, we introduced some adaptions to the protocol.

1. Since Skylinks are not known until after the upload, we use a temporary upload id for all uploads. After the upload is complete, the Syklink can be obtained from the “Upload-Metadata” response header that contains all the TUS-related metadata. The key is “Skylink”, and the value is the base64-encoded Skylink. It is base64-encoded because that’s what TUS requires for its metadata.
2. Skynet uploads data in chunks. The size of these chunks depends on erasure coding settings specified for the fanout and the specified encryption type. The formula for the size of these chunks is `chunkSize := (4MiB — encryptionOverhead) * fanoutDataPieces.` By default, data uploaded to Skynet uses 10 data pieces for its fanout and Threefish for encryption which doesn’t have any overhead. As a result, the default chunk size is 40MiB. Since portals have limited amounts of RAM, they can’t keep these chunks in memory while waiting for users to resume their uploads. That’s why the chunk size specified in TUS needs to be a multiple of the Skynet chunk size. As long as they match, the portal can upload the chunks and free up memory while waiting for more data.

### Conclusion

Adapting the TUS protocol for resumable uploads allows users and developers to upload much larger files to Skynet without worrying about sudden interruptions causing their uploads to fail. Due to the open protocol, developers can leverage a wide ecosystem of existing SDKs and applications for building their Skapps or uploading their collections of large files to Skynet.

To learn more about TUS, visit their official [website](https://tus.io/) or jump straight to their list of existing [implementations](https://tus.io/implementations.html). For example, Uppy lets you fetch data from places such as Dropbox, Instagram, or your local machine and upload them directly to Skynet!

