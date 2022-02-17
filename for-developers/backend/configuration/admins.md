---
description: Description of flags related to admin access on your node.
---

# Admins

## Admin Public Keys

`--admin-public-keys`

Type: String\[]

Default: \[]

A list of public keys which gives users access to the admin panel. If '\*' is specified as a key, anyone can access the admin panel. You can add a space and a comment after every public key and leave a note about who the public key belongs to. Admins can add posts to the global feed, manage the whitelist/blacklist/graylists, among other admin functionality.

## Super Admin Public Keys

`--super-admin-public-keys`

Type: String\[]

Default: \[]

A list of public keys which gives users access to the super admin panel. If '\*' is specified as a key, anyone can access the super admin panel. You can add a space and a comment after every public key and leave a note about who the public key belongs to. Be careful who you grant Super Admin access to - Super admins can manage the reserve price at which you sell DESO, the fee you assess on DESO purchases, among other critical configurations.
