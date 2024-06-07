---
description: Looking for submitting a PR? Thank you!
---

# 💟 Contributing

Here are the few things you need to know to find your way around the project.  \
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
