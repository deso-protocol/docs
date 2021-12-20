---
description: Description of flags related to your node's global state database
---

# Global State

## Global State Remote Node

`--global-state-remote-node`

Type: String

Default: None

The IP:PORT or DOMAIN:PORT corresponding to a node that can be used to set/get global state. When this is not provided, global state is set/fetched from a local DB. Global state is used to manage things like user data, e.g. emails, that should not be duplicated across multiple nodes.

## Global State Remote Secret

`--global-state-remote-secret`

Type: String

Default: None

When a remote node is being used to set/fetch global state, a secret is also required to restrict access.

## Expose Global State

`--expose-global-state`

Type: Boolean

Default: False

If true, other nodes are able to request exposed attributes of your global state. Currently, this allows other nodes to fetch verified usernames, blacklist, graylist, and posts that have been added to the global feed on your node.

## Global State API URL

`--global-state-api-url`

Type: String

Default: None

URL to use to fetch global state data. Only used if expose-global-state is false. If not provided, use own global state. The URL must point to another node that has `true` for its [#expose-global-state](global-state.md#expose-global-state "mention") value. This will allow you to fetch verified usernames, blacklist, graylist, and posts that have been added to the global feed on the requested URL.&#x20;
