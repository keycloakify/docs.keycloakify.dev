---
description: Looking to submit a PR? Thank you!
---

# 💟 Contributing

First, if you're not yet familiar on how to contribute to open source, you might want to watch this short viedo. &#x20;

{% embed url="https://www.youtube.com/watch?v=8lGpZkjnkt4" %}

First thing you want to do is fork the [keycloakify](https://github.com/keycloakify/keycloakify) and the [keycloakify-starter](https://github.com/keycloakify/keycloakify-starter) repos.&#x20;

Here is how you can setup a development environment. You must use Yarn classic (v1).\
First you want to clone keycloakify and the keycloakify starter alongside each other

```bash
cd ~/github
git clone https://github.com/<your github username>/keycloakify
git clone https://github.com/<your github username>/keycloakify-starter
```

You can start the storybook locally in dev mode with:

```bash
cd ~/github/keycloakify
yarn
yarn storybook
```

If you want to test the changes you've made in keycloakify in the starter simply run

```bash
cd ~/github/keycloakify
yarn link-in-starter
```

Once it's watching for changes, in another terminal you can do:

```bash
cd ~/github/keycloakify-starter
yarn storybook
# or
npx keycloakify start-keycloak
```

And see the changes you make on the keycloakify project applied in the starter, it's all hot reloaded.
