# 🔩 Keycloakify in my App

A Keycloakify theme do not need to be a standalone project.\
If you are building an SPA React application  you can install Keycloakify right into your project and build your login pages alongside the other pages of your app.

{% hint style="warning" %}
Keycloakify isn't compatible with [Vite](https://vitejs.dev/) nor [Next.js](https://nextjs.org/) yet. [Webpack is the only bundler supported so far](readme-1.md#supported-meta-framworks). &#x20;

I acknowledge that we need at least Vite support, if it's important for you [get involved in the dicution](https://github.com/InseeFrLab/keycloakify/issues/271). &#x20;
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
