---
description: Collocating your App and your frontend code
---

# 🔩 Keycloakify in my App

A Keycloakify theme do not need to be a standalone project.\
Under certain conditions you can collocate your React app and your Keycloak theme. This enable to design the Keycloak user facing pages like if they where any other page of your project.  (it's what's implemented in [the starter project](https://github.com/keycloakify/keycloakify-starter)).

{% hint style="warning" %}
Currently you can only collocate your Keycloak theme with WebPack SPAs. Typically, [create-react-app](https://create-react-app.dev/) projects.  \
It's not the case of your project? **Don't worry!** You can still use Keycloakify but your theme will need to be a standalone project. Just follow [the instructions to make the starter project standalone](https://github.com/keycloakify/keycloakify-starter#standalone-keycloak-theme).&#x20;

We are working toward making Keycloakify agnostic to the project it's colocated with.  \
This will enable collocation with Vite, Next, Gatsby... [Follow the progress.](https://github.com/keycloakify/keycloakify/pull/275)\

{% endhint %}

Before moving on and setting up Keycloakify in your project, first, mess around with [the starter project](https://github.com/codegouvfr/keycloakify-starter) to familiarize yourself with Keycloakify.

Once you think you are ready to move on:

```bash
yarn add keycloakify
```

add the following script

{% code title="package.json" %}
```json
{
  "scripts": {
     ...
     "prepare": "copy-keycloak-resources-to-public", //This is only for beeing able to test the theme locally in storybook or with an explicit mockPageId
     "build-keycloak-theme": "yarn build && keycloakify"
  }
}
```
{% endcode %}

Git ignore the keycloak build directory:

{% code title=".gitignore" %}
```diff
...
/build_keycloak
```
{% endcode %}

That's it. You can build your App as a Keycloak theme with `yarn build-keycloak-theme`\
Reproduce the directory structure of [the starter project](https://github.com/codegouvfr/keycloakify-starter).

You can eject pages using the `npx eject-keycloak-page` command.&#x20;

You might now want to have a look at the available build options:

{% content-ref url="build-options.md" %}
[build-options.md](build-options.md)
{% endcontent-ref %}

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-keycloakify-in-my-app" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
