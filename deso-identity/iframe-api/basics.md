---
description: Guide on integrating with the DeSo Identity iframe API
---

# Overview

The iframe allows you to perform actions such as signing transactions or encrypting/decrypting messages without user interaction, as is the case in the [window-api](../window-api/ "mention").

The iframe is usually entirely invisible to the user.

However, the iframe does need to render on some browsers such as Safari as the user needs to click on the iframe to grant it storage access.

We will explain how to implement this later in the guide.

In order to communicate with the iframe API, you need to include the iframe window HTML component into your app and point it to the `https://identity.deso.org/embed` URL.

Below is an example component.

We also provided you with an example with CSS styling.

```markup
<iframe
  id="identity"
  frameborder="0"
  src="https://identity.deso.org/embed"
  style="height: 100vh; width: 100vw; display: none; position: fixed; 
    z-index: 1000; left: 0; top: 0;"
></iframe>
```

Take notice of the CSS styling of the iframe component. As mentioned, the iframe is typically invisible to the user. That's why we set the `display: none` in the CSS style.

We also want the iframe window to be on top of your application and take the entire display, hence the other CSS attributes.

You should modify the `z-index` attribute to fit your application. In case that we will have to show the iframe to the user, you will set the attribute `display: block`

For example, `document.getElementById("identity").style.display = "block"`

## Messages

Communication with the iframe context is done through sending `postMessage()` requests. Similarly, the iframe context will send you responses by sending message events.

When you open the iframe context for the first time, the Identity will send you an `initialize` message, which requires you to respond.

This concept has been explained in more detail in the [#events](../identity/concepts.md#events "mention") section.

When sending requests to the Identity iframe, you will need to include an `id` field. The `id` should be in [UUID v4](https://en.wikipedia.org/wiki/Universally\_unique\_identifier#Version\_4\_\(random\)) format.

We recommend using the [uuid npm package](https://www.npmjs.com/package/uuid), or you can also use the following implementation in Vanilla JavaScript:

```javascript
function uuid() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}
```

Here's an example message format that you can send to the Identity iframe.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'info',
}
```

Let's take a look at what each of these fields mean:

* `id` should be generated to follow the UUID v4 format.
* `service` this should always be set to `"identity"` so that the DeSo Identity Service knows it's supposed to read this message.
* `method` this is the API type that you'll request, in this case it's `"info"` but it could be`"sign"`, `"encrypt"`, etc. as outlined in the [#api](./#api "mention") section

You should index requests by `id` so that you can match them with responses.

We recommend looking at the [DeSo Protocol sourcecode](https://github.com/deso-protocol/frontend) to see how this could be implemented.&#x20;

In particular, you could trace through the `send()` method on [line #257 in `src/app/identity.service.ts`](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L257).

## Storage Access

Besides `initialize`, you will need to send an `info` message to the `iframe` context.

The DeSo Identity uses local storage and cookies to share information about users between the window and iframe contexts.

However, some browsers such as Safari and Chrome on iOS prevent storing data in such a way without user's explicit permission.

For example, Apple's Intelligent Tracking Prevention (ITP) places strict limitations on cross-domain data storage and access.

This means the Identity iframe must request storage access every time the page reloads.

This can be accomplished by showing the iframe to the user, i.e. setting `display: "block"`, and having the user click on the iframe.

When a user visits a DeSo application in Safari they will see a "Tap anywhere to unlock your wallet" prompt which is a giant button in the iframe. When the user clicks on the button, he will give Identity the required access.

Here's how it looks for the user:

![iframe UI for granting storage access](<../../.gitbook/assets/Screenshot from 2021-11-28 15-45-23.png>)

To simplify this process, there's a special API call that you should perform, called `info`, that will indicate if you should display the iframe window to the user.

The best practice is to send the `info` message just when you're about to send the first non-`initialize` request (sign, encrypt, etc.) to the iframe API.

We recommend tracing through the `identity/iframe-info-storage-access` code snippet in the [examples repository,](https://github.com/deso-protocol/examples/tree/main/identity/iframe-info-storage-access/) which implements this communication pattern in Vanilla JavaScript using promises.

If you're familiar with [RxJS](https://rxjs.dev), you can also trace through the [Deso Protocol code](https://github.com/deso-protocol/frontend), starting with the `storageGranted` variable on [line #32](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L32). Here's how the `info` request looks like:

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'info',
}
```

And here's the response that the iframe will send you:

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    browserSupported: true,
    hasCookieAccess: true,
    ​​hasLocalStorageAccess: true,
    ​​hasStorageAccess: true
  },
}
```

Let's take a look at the above payload fields:

* `browserSupported` tells you if DeSo Identity Service is compatible with the user's browser. If it's set to `false` you should notify the user that Identity won't work for them. Most modern browsers will output `true`.
* `hasCookieAccess` indicates if user browser allows cookies. Identity might store some information in cookies if local storage is unavailable.
* `hasLocalStorageAccess` will be `true` if local storage is available on user's browser.
* `hasStorageAccess` indicates if browser has access to storage. If it's set to `false` you will need to show the `iframe` window to the user so he can grant access. We will get to this next.

If the `info` message returns `hasStorageAccess: false`, your application should make the iframe take over the entire page.

You could do that by setting:

```javascript
document.getElementById("identity").style.display = "block"
```

The `info` message also detects if a user has disabled third party cookies.

Third party cookies are required for Identity to securely sign transactions.

If neither localStorage nor cookies are available, the `info` returns `browserSupported: false` and your application should inform the user they will not be able to use Identity to sign or decrypt anything.

When a user clicks "Tap anywhere to unlock your wallet," the iframe will indicate it by sending a `storageGranted` message.&#x20;

This request does not expect a response. When your application receives the `storageGranted` message it can hide the `iframe` window from the user and the iframe is now ready to receive `sign` and `decrypt`, etc. messages.

To hide the `iframe`, simply call:

```javascript
document.getElementById("identity").style.display = "none"
```

#### `storageGranted` Message

```javascript
{
  service: 'identity',
  method: 'storageGranted',
}
```

## User Credentials

In order to use the iframe API, you will need to first acquire secure user credentials from the [window-api](../window-api/ "mention"), through the [#log-in](../window-api/#log-in "mention") endpoint.

The user credentials include three fields, namely `encryptedSeedHex,` `accessLevel`, and`accessLevelHmac`.

Which you have to include in all requests to the iframe API. We also explained this concept in the [#accounts](../identity/concepts.md#accounts "mention") section.
