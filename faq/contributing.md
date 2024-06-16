---
description: Looking to submit a PR? Thank you!
---

# 💟 Contributing

Here is how you can setup a devlopement environement. You must use Yarn classic (v1).    \
First you want to clone keycloakify and the keycloakify starter alongside each other

```bash
cd ~/github
git clone https://github.com/keycloakify
git clone https://github.com/keycloakify-starter
```

You can start the storybook locally in dev mode with: &#x20;

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

After that you can

```bash
cd ~/github/keycloakify-starter
yarn storybook # or
npx keycloakify start-keycloak
```

And see the changes you make on the keycloakify project applied in the starter. &#x20;
