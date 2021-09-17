# Privacy and Security

## **What is a seed phrase?**

Your seed phrase is the password that controls your entire account. **Your seed phrase can never be changed and sharing your seed phrase with anyone could result in total loss of funds.**

## **Does bitclout.com have access to my seed phrase?**

**No, bitclout.com does not have access to your seed phrase.** Your seed is stored in your browser in a highly-secured `iframe` that is completely isolated from the rest of the app. Transactions are signed in your browser by this `iframe` and **your seed phrase never leaves your browser.**

## **How do I keep my account safe?**

**For maximum security, the developer community recommends you use mobile browsers or the DeSo desktop app to access bitclout.com.** 

Both mobile browsers and the DeSo desktop app are secured, sandboxed environments that cannot be easily compromised by third party attackers.

While bitclout.com is generally safe to access in a regular Desktop browser like Chrome, Safari, Firefox, etc, there is some risk that malicious browser extensions could steal your seed phrase if you give them access to bitclout.com. **This should be rare, but if you own a large amount of DeSo it is recommended that you either disable extensions manually or use Incognito mode, which disables extensions automatically.**

Furthermore, if you own a large amount of DeSo it may be prudent to create two separate accounts: one account that you use to post and follow, and a separate account to hold your coins. 

## **What do I do i**f I lose my seed phrase?

Because your seed phrase is stored exclusively in your browser, it cannot be recovered by any third party, including bitclout.com. **If you lose your seed phrase, currently the only option is to create a new account, save the new seed phrase, and send all of your holdings to this new account.** You may need to liquidate your creator coin holdings into DeSo first. You can transfer a username by first changing the username on the old account and then quickly claiming the username with the new account.

We apologize for the inconvenience here. We know this process is not ideal, we're working on alternative solutions, and hope to have an easier recovery path for users in the future.

## How does DeSo Identity work?

DeSo Identity, located at [identity.deso.org](https://identity.deso.org), safely stores your sensitive account information in your browser's local storage. To protect private key material the identity service has minimal dependencies, a strict content security policy, and is audited by multiple security firms. **For now, the developer community does not recommend entering your seed phrase anywhere other than identity.deso.org.** Always check the URL bar to verify you are using identity.deso.org.

The DeSo Identity Service aims to make it easy for users to use a wide array of community projects safely and securely. Apps and nodes can integrate with DeSo Identity to onboard users without requiring them to enter private key material. Users can easily grant different levels of access on a per-account basis. The developer community is working on complete documentation for integrating with the DeSo Identity Service.

