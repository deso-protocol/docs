---
description: Description of common data types that appear in the Transaction endpoints
---

# Data Types

## TransactionFee

`TransactionFees` are additional transaction outputs.  These additional outputs are a way for both node operators (who can specify additional fees for all transactions of a certain type on their node) and app developers (who can specify additional fees when make a request to construct a transaction).

```json5
{
  "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user who will receive the additional output,
  "ProfileEntryResponse": <ProfileEntryResponse>, // This is only provided when TranssactionFees are retrieved through admin endpoints when managing node-leve transaction fees
  "AmountNanos": 10000, // The amount of DeSo in nanos this user should receive
}
```

## AccessGroupMember

`AccessGroupMembers`  are objects used when performing Access Group Member transactions which add, remove, or update members of an access group. These objects are only using in the construction of these transaction.

#### Attributes

* AccessGroupMemberPublicKeyBase58Check: the public key of the user who is being added, removed, or updated
* AccessGroupMemberKeyName: the access group key name belong to AccessGroupMemberPublicKeyBase58Check by which the user is being added to or removed from the group. &#x20;
* EncryptedKey: the private key of the access group is encrypted to the AccessGroupPublicKey of the member's access group. This must be left empty when removing a member from a group
* ExtraData: arbitrary key value data used to add details about this access group member&#x20;

```json5
{
  "AccessGroupMemberPublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s",
  "AccessGroupMemberKeyName": "default-key",
  "EncryptedKey": "someencryptedhexstring",
  "ExtraData": { "key": "value" },
}
```

For reference, `TransactionFee` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/admin\_fees.go#L16).
