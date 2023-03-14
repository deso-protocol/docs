---
description: Get started building on DeSo with our Javascript SDK
---

# 1⃣ Frontend: Get Started

### Setup

If you’re already familiar with a particular framework, feel free to set up a project using the documentation for your preferred tool (Create React App, Vite, Nextjs, Remix, Angular, Vue, etc).

If you're not sure, [our examples](https://github.com/deso-protocol/deso-examples-react) will be using [Create React App](https://create-react-app.dev/), which is a reasonable choice for getting a development environment up and running for quick prototyping.\
\
You can find the system requirements and installation steps on the Create React App [getting started page](https://create-react-app.dev/docs/getting-started).

Note that CRA’s default scaffolding uses vanilla javascript.\
\
If you’re comfortable with typescript and prefer to use it, [see this section](https://create-react-app.dev/docs/getting-started#creating-a-typescript-app) to get started. Otherwise, using vanilla javascript is a reasonable choice.

Once you have your app up and running with the default scaffolding, we’ll install the DeSo identity package and look at some examples.

### Installation

[The DeSo Protocol SDK](https://www.npmjs.com/package/deso-protocol) is used for core functionality when building an application for the DeSo blockchain: logging in, logging out, signing and submitting transactions, and more. \
\
NPM: [https://www.npmjs.com/package/deso-protocol](https://www.npmjs.com/package/deso-protocol)\
Github: [https://github.com/deso-protocol/deso-workspace/tree/main/libs/deso-protocol](https://github.com/deso-protocol/deso-workspace/tree/main/libs/deso-protocol)

\
In the root of your application run:

```
npm i deso-protocol
```

And now you should be ready to start building!

## Configuration

Change the identity library’s behavior based on the needs of your app.

### Transaction spending limit options

Setting transaction spending limit options will determine what permissions your users will see when logging into your app, and the amount in DeSo your app can spend on their behalf.

On DeSo, every user gets a main or _**owner**_ keypair generated for them when they create an account for the first time.\
\
The owner key is able to sign _**any**_ transaction on behalf of the user, including spending _**all**_ of their money, and transferring **all** of their NFTs or tokens!

It wouldn’t be a good idea to give every app the user interacts with direct access to this key. But we also don’t want to make users have to manually approve _every single transaction_ that an app wants them to do.\
\
Can you imagine using Twitter if every like and comment required an annoying approval popup?

The solution is to allow apps to generate **subkeys** or _**derived**_ keys that have a limited set of permissions, approved by the _**owner**_ key.\
\
This looks as follows:

* User creates an account for the first time, generating an _**owner**_ public/private keypair that is stored in DeSo Identity (stored locally in the browser, but on a distinct domain that is not accessible to apps).\

* User gets some starter $DESO coins to cover gas, either by entering their phone number or buying some.\

* App generates a _**derived**_ key, which can just be any random keypair, and a transaction granting this derived key certain permissions, e.g. the ability to post 3 times on the user’s behalf.\

* Once the derived key approval transaction is generated, the DeSo Identity wallet can prompt the user to _**approve**_ it, thus signing the transaction with the user’s _**owner**_ public key.\

* Once an approve txn is signed by the user’s owner public key, it can be _**broadcast**_ to the DeSo blockchain, which then gives the _**derived**_ key the desired permissions.\

* The app can then happily sign transactions on the user’s behalf, without the user having to worry about the app stealing their funds (also known as getting **rug-pulled** or **rugged**).

In this example, we will ask users for permission to create an unlimited number of posts and make an unlimited number of transfers until they meet the global limit of 1 $DESO.

```
import { configure } from 'deso-protocol';

configure({
  spendingLimitOptions: {
    // NOTE: this value is in Deso nanos, so 1 Deso * 1e9
    GlobalDESOLimit: 1 * 1e9 // == 1 Deso
    // Map of transaction type to the number of times this derived key is
    // allowed to perform this operation on behalf of the owner public key
    TransactionCountLimitMap: {
      BASIC_TRANSFER: 'UNLIMITED', // Sending/receiving DESO is a "basic transfer"
      SUBMIT_POST: 'UNLIMITED',
    },
  }
});
```

**Important:** You’ll want to make sure you call configure only once prior to calling any other identity methods.

Note that even though we approve an unlimited number of **basic transfer** transactions in the above configure() call, we cannot take more than 1 $DESO from the user’s wallet before we have to pop up an approval again.

This is a good thing for the user!

While you’ll typically want to ask for specific permissions in a production app, it is possible to ask for unlimited access for prototyping or quickly testing things.\
\
This example requests approval for unlimited access:

```
import { configure } from 'deso-protocol';

configure({
  spendingLimitOptions: {
    IsUnlimited: true
  }
});
```

### App Name

`appName` is used to identify the app that authorizes a derived key.\
\
This can be used to group and identify derived keys that have been issued by a given app. If you don’t set the appName, the domain name your app is running on will be used by default.

```
import { configure } from 'deso-protocol';

configure({
  appName: 'My Cool App',
  spendingLimitOptions: {
    IsUnlimited: true
  }
});
```

Soon, users will be able to easily see all the apps they’ve used, and what permissions they’ve granted them (this is why setting a good app name is helpful!).\
\
Users will also be able to disable permissions from one unified dashboard.

### Next steps

Next, we’ll look at some basic [examples of common scenarios.](https://github.com/deso-protocol/deso-examples-react)
