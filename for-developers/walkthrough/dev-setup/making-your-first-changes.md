# Making Your First Changes

In this tutorial, we will show you how to make changes to the DeSo codebase, and see your changes reflected in a local dev environment.

## Prerequisites

This guide assumes you have successfully made it through [**Setting Up Your Dev Environment**](./). In particular, it assumes you have a testnet node running with n0\_test showing a frontend UI that looks roughly like the following screenshot:

![](../../../.gitbook/assets/desopage1.png)

## Make Your First Frontend Change

If your frontend repo is loaded into Goland, the following steps should allow you to make your first frontend change, and see it update your local node in real time:

* Run your n0\_test. Create an account and make sure you can see the page shown in the prerequisites.
* Assuming you're using Goland, navigate to the `feed.component.ts`. Hint: You can use SHIFT+SHIFT to easily jump to it.
* Look for the `GLOBAL_TAB` function in the file and modify the return statement as follows (you can name your feed whatever you want):
  * `static GLOBAL_TAB = "Satoshi's Feed";`
* Save your changes.

After your changes are saved, your browser should update to show a new title for your feed tab:

![](../../../.gitbook/assets/desopage2.png)

## Make Your First Backend Change

The backend repo runs an API that the frontend Angular app queries to get all of the information it displays. Let's make our first change to this API by following the steps below:

* Before going into the code, go to the Admin panel, add a post to the global feed, and verify that it shows up by refreshing the page.
* With the backend repo loaded in Goland, find the `post.go` file, which defines one of the API endpoints queried by the frontend. Hint: You can use SHIFT+SHIFT to navigate to it.
* In that file, find a function called `GetPostsStateless`. Modify the response at the end of the function as follows to customize the content:
  * ```
        if len(postEntryResponses) > 0 {
            postEntryResponses[0].Body = "This is some content"
        }

        // Return the posts found.
        res := &GetPostsStatelessResponse{
            PostsFound: postEntryResponses,
        }
    ```
* Save the file and restart n0\_test. When you make changes to anything in backend or core, you need to restart your node for them to take effect.

Now you should see some custom content in the post that you added to the feed. You can modify endpoints in backend like this one to customize how data is returned to the user.

![](../../../.gitbook/assets/desopage3.png)

## Make Your First Core Change

Your first core change will consist of adding the support of emoji reactions to posts.
We will implement this change by creating a new transaction from scratch.

### Step 1: Development

First, navigate to the `network.go` file in the `lib` folder.
The `network.go` file defines all the data structures used to form both messages and transactions over the network.
The first set of changes consists of adding constants to identify the new transaction type you are creating.

In `network.go`, perform the following changes:
* Add a new `TxnType` constant named `TxnTypeReact` and update the value of `NEXT_ID` to `27`.
```go
TxnTypeDAOCoinTransfer              TxnType = 25
TxnTypeReact                        TxnType = 26

// NEXT_ID = 27
)
```
* Add a new `TxnString` constant named `TxnStringReact`.
```go
TxnStringDAOCoinTransfer              TxnString = "DAO_COIN_TRANSFER"
TxnStringReact                        TxnString = "REACT"
TxnStringUndefined                    TxnString = "TXN_UNDEFINED"
)
```

Now, update functions, methods, and constants that use these constant values.

* Append the `TxnTypeReact` value to the `AllTxnTypes` `[]TxnType` slice.
```go
AllTxnTypes = []TxnType{
TxnTypeUnset, ...,
TxnTypeDAOCoin, TxnTypeDAOCoinTransfer, TxnTypeReact,
}
```
* Append the `TxnStringReact` value to the `AllTxnString` `[]TxnString` slice.
```go
AllTxnString = []TxnString{
TxnStringUnset, ...,
TxnStringDAOCoin, TxnStringDAOCoinTransfer, TxnStringReact,
}
```
* In `GetTxnString`, add a case for the `TxnTypeReact`.
```go
case TxnTypeDAOCoinTransfer:
    return TxnStringDAOCoinTransfer
case TxnTypeReact:
    return TxnStringReact
default:
    return TxnStringUndefined
```
* (_Optional_) In `GetTxnTypeFromString`, add a case for the `TxnStringReact`.
```go
case TxnStringDAOCoinTransfer:
    return TxnTypeDAOCoinTransfer
case TxnStringReact:
    return TxnTypeReact
default:
    // TxnTypeUnset means we couldn't find a matching txn type
    return TxnTypeUnset
}
```

Next, create a new transaction metadata type that records emoji reactions.
In this example, you can assume that a single transaction contains only one emoji reaction.
Multiple reaction to a single post would correspond to multiple transactions.

* At the end of the file, insert the following code block.
The new struct has a rune field to store the Unicode of the emoji reaction.
The `ToBytes` method validates the post hash and
then builds a byte slice containing both the validated post hash and the normalized NFC Unicode emoji reaction.
* Why use a normalized NFC Unicode?
It is possible for a single character to be encoded with different code point sequences.
By normalizing the unicode, we ensure that a character will have a unique code point sequence.

```go
// ==================================================================
// ReactMetadata
//
// A reaction is an interaction where a user on the platform reacts to a post.
// ==================================================================

type ReactMetadata struct {
    // The user reacting is assumed to be the originator of the
    // top-level transaction.
    
    // The post hash to react to.
    PostHash *BlockHash
    
    EmojiReaction rune
}

func (txnData *ReactMetadata) GetTxnType() TxnType {
    return TxnTypeReact
}

func (txnData *ReactMetadata) ToBytes(preSignature bool) ([]byte, error) {
    // Validate the metadata before encoding it.
    //
    // Post hash must be included and must have the expected length.
    if len(txnData.PostHash) != HashSizeBytes {
        return nil, fmt.Errorf("ReactMetadata.ToBytes: PostHash "+
        "has length %d != %d", len(txnData.PostHash), HashSizeBytes)
    }

    var data []byte
    
    // Add PostHash
    //
    // We know the post hash is set and has the expected length, so we don't need
    // to encode the length here.
    data = append(data, txnData.PostHash[:]...)
    
    // Add EmojiReaction.
    data = append(data, norm.NFC.Bytes([]byte(string(txnData.EmojiReaction)))...)
    
    return data, nil
}

func (txnData *ReactMetadata) FromBytes(data []byte) error {
    ret := ReactMetadata{}
    rr := bytes.NewReader(data)
    
    // LikedPostHash
    ret.PostHash = &BlockHash{}
    _, err := io.ReadFull(rr, ret.PostHash[:])
    if err != nil {
        return fmt.Errorf(
        "ReactMetadata.FromBytes: Error reading PostHash: %v", err)
    }
    
    // Emoji reaction
    reaction, _, err := rr.ReadRune()
    if err != nil {
        return fmt.Errorf(
        "ReactMetadata.FromBytes: Error reading EmojiReaction: %v", err)
    }
    
    ret.EmojiReaction = reaction
    *txnData = ret
    
    return nil
}

func (txnData *ReactMetadata) New() DeSoTxnMetadata {
    return &ReactMetadata{}
}
```

The last step before testing is to add this new transaction type metadata to the `NewTxnMetadata` function.
* In `NewTxnMetadata`, add a case for `TxnTypeReact`.

```go
case TxnTypeDAOCoinTransfer:
    return (&DAOCoinTransferMetadata{}).New(), nil
case TxnTypeReact:
    return (&ReactMetadata{}).New(), nil
default:
    return nil, fmt.Errorf("NewTxnMetadata: Unrecognized TxnType: %v; make sure you add the new type of transaction to NewTxnMetadata", txType)
}
```

### Step 2: Testing
Add the two tests below at the bottom of the `network_test.go` file.
The first test attempts to create a default `ReactMetadata` both manually and then by calling the `NewTxnMetadata` function.
After that, it attempts to convert `ReactMetadata` to bytes and then back to `ReactMetadata`.
The second test performs similar actions but specifies the emoji reaction to use.

```go
func TestSerializeNoReaction(t *testing.T) {
    require := require.New(t)
    
    txMeta := &ReactMetadata{PostHash: &postHashForTesting1}
    
    data, err := txMeta.ToBytes(false)
    require.NoError(err)
    
    testMeta, err := NewTxnMetadata(TxnTypeReact)
    require.NoError(err)
    err = testMeta.FromBytes(data)
    require.NoError(err)
    require.Equal(txMeta, testMeta)
}

func TestSerializeReactions(t *testing.T) {
    ValidReactions := []rune{'😊', '😥', '😠', '😮'}
    for _, r := range ValidReactions {
    _testSerializeSingleReaction(t, r)
    }
}

func _testSerializeSingleReaction(t *testing.T, emoji rune) {
    require := require.New(t)
    
    txMeta := &ReactMetadata{
    PostHash:      &postHashForTesting1,
    EmojiReaction: emoji,
    }
    
    data, err := txMeta.ToBytes(false)
    require.NoError(err)
    
    testMeta, err := NewTxnMetadata(TxnTypeReact)
    require.NoError(err)
    err = testMeta.FromBytes(data)
    require.NoError(err)
    require.Equal(txMeta, testMeta)
}
```

Ensure you can run the tests successfully, and you are good to go!