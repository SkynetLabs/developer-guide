# Data Access Controllers

## Commonly Used DACs

The DACs commonly in use today implement data schemas developed in the community-organized [Skystandards repository](https://github.com/SkynetLabs/skystandards). The DAC libraries let application developers quickly implement and access MySky files adhering to these standards for seamless interoperability between Skynet applications. 

* \*\*\*\*[**Feed DAC**](https://github.com/redsolver/feed-dac-library) – Implements the [Feed](https://github.com/SkynetLabs/skystandards/blob/main/feed-page/README.md) and [Post](https://github.com/SkynetLabs/skystandards/blob/main/feed-page/post/README.md) Skystandards. Despite their name, these schema can represent a versatile range of data, ranging from social posts, to blog articles, to [chess matches](https://skychess.hns.siasky.net/).
* \*\*\*\*[**Social DAC**](https://github.com/redsolver/social-dac-library) – Implements the [User Relations](https://github.com/SkynetLabs/skystandards/blob/main/user-relations/README.md) Skystandard. This lets a user "follow" other MySky users and discover their published data across applications. Can be used for a Friends List or Social Graph.
* \*\*\*\*[**User Profile DAC**](https://github.com/skynethubio/userprofile-library) –Implement the [Profile](https://github.com/SkynetLabs/skystandards/blob/main/profile/README.md) Skystandard. Allows a user to set a user profile and preferences that extend across the entire MySky ecosystem.

{% hint style="info" %}
MySky and DACs were debuted with a hackathon, where a reference DAC call the [Content Record DAC](https://github.com/SkynetLabs/content-record-library) was provided for the community to build with. You may see it referenced, but it's not as powerful at representing complex data as later builds like the Feed DAC.
{% endhint %}

