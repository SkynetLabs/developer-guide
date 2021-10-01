# Kratos and Oathkeeper

[Kratos](https://www.ory.sh/kratos) is our user management system of choice and [Oathkeeper](https://www.ory.sh/oathkeeper) is the identity and access proxy.

Most of the needed config is already under `docker/kratos`. The only two things that need to be changed are the config for Kratos that might contain your email server password, and the JWKS Oathkeeper uses to sign its JWT tokens.

Make sure to create your own`docker/kratos/config/kratos.yml` by copying the `kratos.yml.sample` in the same directory. Also, make sure to never add that file to source control because it will most probably contain your email password in plain text!

## New Cluster Set Up

If you are setting up a new cluster then you will need to generate a new set of keys. 

To override the JWKS you will need to directly edit `docker/kratos/oathkeeper/id_token.jwks.json` and replace it with your generated key set. If you don't know how to generate a key set you can use this code:

```go
package main

import (
    "encoding/json"
    "log"
    "os"

    "github.com/ory/hydra/jwk"
)

func main() {
    gen := jwk.RS256Generator{
        KeyLength: 2048,
    }
    jwks, err := gen.Generate("", "sig")
    if err != nil {
        log.Fatal(err)
    }
    jsonbuf, err := json.MarshalIndent(jwks, "", "  ")
    if err != nil {
        log.Fatal("failed to generate JSON: %s", err)
    }
    os.Stdout.Write(jsonbuf)
}
```

While you can directly put the output of this program into the file mentioned above, you can also remove the public key from the set and change the `kid` of the private key to not include the prefix `private:`.

{% hint style="info" %}
Make sure to save the `id_token.jwks.json` file somewhere safe like LastPass. This file will be reused on all other servers in your portal cluster. 
{% endhint %}

## Adding to a Cluster

If you are adding a new node to an existing cluster then you will be using the keys generated when the cluster was initialized. Copy the common `id_token.jwks.json` file to `skynet-webportal/docker/kratos/oathkeeper/id_token.jwks.json`

