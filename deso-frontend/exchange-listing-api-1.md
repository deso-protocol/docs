---
description: Use this React example to start building your first app on DeSo
---

# 2⃣ Frontend: React Example

This is a simple [Create React App](https://create-react-app.dev/docs/getting-started) project, but these examples can be easily ported to your preferred framework or build tool.

Github: [https://github.com/deso-protocol/deso-examples-react](https://github.com/deso-protocol/deso-examples-react)

### How to run these examples locally

Run the following in your terminal

```
git clone https://github.com/deso-protocol/deso-examples-react.git
cd deso-examples-react
npm i
npm run start
```

### How to use this repository

If you want to port these examples to your own app, set up a project using the docs for your preferred tool (Create React App, Vite, Nextjs, Remix, Angular, Vue, etc).

If you're not sure, Create React App is a reasonable choice for getting a development environment up and running for quick prototyping/experimenting.

Next, install the [DeSo identity library](https://www.npmjs.com/package/@deso-core/identity) using your preferred package manager:

```
# npm
npm i @deso-core/identity

# yarn
yarn add @deso-core/identity
```

Finally, use the examples found in this repo to help you build social features for your application using the [DeSo blockchain](https://deso.com/)

There are lots of comments throughout the code, but if anything is unclear, please open an issue!

### Examples

* [Configuration](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/root.jsx#L7)
* [Login](https://github.com/deso-protocol/deso-examples-react/blob/main/src/components/nav.jsx#L27)
* [Logout](https://github.com/deso-protocol/deso-examples-react/blob/main/src/components/nav.jsx#L31)
* State Sync
  1. [Create a react context](https://github.com/deso-protocol/deso-examples-react/blob/main/src/contexts.js#L7)
  2. [Set up useState hook](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/root.jsx#L18)
  3. [Set up useEffect hook](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/root.jsx#L24)
  4. [Subscribe to identity](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/root.jsx#L40)
  5. [Instantiate a context provider](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/root.jsx#L117)
  6. [Use state from identity anywhere](https://github.com/deso-protocol/deso-examples-react/blob/main/src/components/nav.jsx#L8)
  7. [React to changes in your code](https://github.com/deso-protocol/deso-examples-react/blob/main/src/components/nav.jsx#L16)
* [Check permissions](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/sign-and-submit-tx.jsx#L8)
* [Request permissions](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/sign-and-submit-tx.jsx#L50)
* [Create, sign, submit a transaction](https://github.com/deso-protocol/deso-examples-react/blob/main/src/routes/sign-and-submit-tx.jsx#L61)

