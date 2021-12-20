---
description: Description of flags related to your node's web security
---

# Web Security

## Access Control Allow Origins

`--access-control-allow-origins`

Type: String\[]

Default: \["\*"]

Accepts a comma-separated lists of origin domains that will be allowed as the Access-Control-Allow-Origin HTTP header. Defaults to \* if not set which allows all origin domains.

## Secure Header Development

`--secure-header-development`

Type: Boolean

Default: true

If set, runs our secure header middleware in development mode, which disables some of the options. The default is true to make it easy to run a node locally. See [https://github.com/unrolled/secure](https://github.com/unrolled/secure) for more info.

## Secure Header Allow Hosts

`--secure-header-allow-hosts`

Type: String\[]

Default: \[],

This is the domain that our secure middleware will accept requests from. We also set the HTTP Access-Control-Allow-Origin

##
