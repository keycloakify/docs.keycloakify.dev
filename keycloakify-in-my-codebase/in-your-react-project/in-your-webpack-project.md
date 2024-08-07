# In your Webpack Project

If you have a Webpack/React/TypeScript project you can integrate Keycloakify directly inside it.

In this guide we're going to work with a vanilla [Create React App](https://create-react-app.dev/) project.

<figure><img src="../../.gitbook/assets/image (59).png" alt="" width="375"><figcaption><p>Creating a CRA project. You don't need to do that, just use your existing codebase.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (60).png" alt="" width="304"><figcaption><p>Our codebase before involving Keycloakify</p></figcaption></figure>

{% hint style="info" %}
Before anything make sure to commit all your pending changes so you can easily revert changes if need be.
{% endhint %}

Let's start by installing Keycloakify (and optionally Storybook) to our project:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add keycloakify@next
yarn add --dev rimraf storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add keycloakify@next
pnpm add --dev rimraf storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="bun" %}
```bash
bun add keycloakify@next
bun add --dev rimraf storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install --save keycloakify@next
npm install --save-dev rimraf storybook @storybook/react @storybook/react-vite
```
{% endtab %}
{% endtabs %}

Next we want to repatriate the relevant files from [the starter template](https://github.com/keycloakify/keycloakify-starter) into our project:

```bash
cd my-app
git clone https://github.com/keycloakify/keycloakify-starter-webpack tmp
mv tmp/src src/keycloak-theme
mv tmp/.storybook .
rm -rf tmp
rm src/keycloak-theme/react-app-env.d.ts
mv src/keycloak-theme/index.tsx src/index.tsx
```

<figure><img src="../../.gitbook/assets/image (61).png" alt="" width="308"><figcaption><p>Sate of your codebase after bringing in Keycloakify's starter boilerplate code</p></figcaption></figure>

Now you want to modify your entry point so that:

* If the kcContext global is defined, render your Keycloakify theme
* Else, render your App as usual.

<pre class="language-tsx" data-title="src/index.tsx"><code class="lang-tsx">/* eslint-disable react-refresh/only-export-components */
import { createRoot } from "react-dom/client";
import { 
    StrictMode,
<strong>    lazy,
</strong><strong>    Suspense
</strong>} from "react";
<strong>import { KcPage, type KcContext } from "./keycloak-theme/kc.gen";
</strong><strong>const App = lazy(()=> import("./App"));
</strong>
<strong>// The following block can be uncommented to test a specific page with `yarn dev`
</strong><strong>// Don't forget to comment back or your bundle size will increase
</strong><strong>/*
</strong><strong>import { getKcContextMock } from "./keycloak-theme/login/KcPageStory";
</strong>
<strong>if (import.meta.env.DEV) {
</strong><strong>    window.kcContext = getKcContextMock({
</strong><strong>        pageId: "register.ftl",
</strong><strong>        overrides: {}
</strong><strong>    });
</strong><strong>}
</strong><strong>*/
</strong>
createRoot(document.getElementById("root")!).render(
    &#x3C;StrictMode>
<strong>        {window.kcContext ? (
</strong><strong>            &#x3C;KcPage kcContext={window.kcContext} />
</strong><strong>        ) : (
</strong><strong>            &#x3C;Suspense>
</strong><strong>                &#x3C;App />
</strong><strong>            &#x3C;/Suspense>
</strong><strong>        )}
</strong>    &#x3C;/StrictMode>
);

<strong>declare global {
</strong><strong>    interface Window {
</strong><strong>        kcContext?: KcContext;
</strong><strong>    }
</strong><strong>}
</strong></code></pre>

{% hint style="info" %}
**Question:**

Why do my main application and Keycloak theme share the same entry point?

**Answer:**

To simplify the build process. If you don't want it to negatively impact the performance of your application, it's essential to understand the following points:

* **Different Contexts:** The application (`App`) and Keycloak page (`KcPage`) are mounted in very different contexts. Avoid sharing providers between the two at the `main.tsx` file level. The true entry point of your application is the `App` component, while the entry point for your Keycloak theme is the `KcPage` component. Be careful about what code is shared between them.
* **Responsibility of main.tsx:** The `main.tsx` file should only determine the context (either the application or Keycloak) and mount the appropriate component (`App` or `KcPage`). It should not contain any substantial logic or dependencies.
* **Performance Considerations:** Keep `main.tsx` as lightweight as possible to avoid increasing the initial load time of both your main application and login pages. For example, do not load any state management libraries like `redux-toolkit` at this level. Only the main application needs `redux-toolkit`, not the login pages.

By following these guidelines, you can ensure a clear separation of contexts and maintain optimal performance for both your main application and Keycloak theme.
{% endhint %}

Finally you want to add some script for Keycloakify in you package.json and also let Keycloakify know about how your Webpack project is configured.

<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "name": "my-app",
    "scripts": {
<strong>        "prestorybook": "keycloakify update-kc-gen &#x26;&#x26; keycloakify copy-keycloak-resources-to-public",
</strong><strong>        "storybook": "storybook dev -p 6006",
</strong><strong>        "prestart": "npm run prestorybook",
</strong>        "start": "react-scripts start",
<strong>        "prebuild": "keycloakify update-kc-gen",
</strong>        "build": "react-scripts build",
<strong>        "postbuild": "rimraf build/keycloak-resources",
</strong><strong>        "build-keycloak-theme": "npm run build &#x26;&#x26; keycloakify build",
</strong>        // ...
    },
<strong>    "keycloakify": {
</strong><strong>        "accountThemeImplementation": "none",
</strong><strong>        "projectBuildDirPath": "build",
</strong><strong>        "staticDirPathInProjectBuildDirPath": "static",
</strong><strong>        "publicDirPath": "public"
</strong><strong>    },
</strong>    // ...
</code></pre>

{% hint style="info" %}
Leave accountThemeImplementation set to "none" for now.\
To initialize the account theme refer to [this guide](../../keycloak-configuration/enabling-your-theme/account-theme.md).
{% endhint %}

Keycloakify has many build options that you can use, however `projectBuildDirPath`, `staticDirPathInProjectBuildDirPath` and `publicDirPath` are parameters specific to the use of Keycloakify in a Webpack context.

Theses **are not preferences!** If you're not using Create React App your Webpack configuration is probably different and you want to update those values to reflect how webpack build your site in your project.

<figure><img src="../../.gitbook/assets/image (62).png" alt="" width="209"><figcaption><p>Here you can see that in a CRA project, when we run <code>npm run build</code> the app distribution is generated in a <strong>build/</strong> directory, this is why we use <code>"projectBuildDirPath": "build"</code>. We can also see that all the assets of the app are gathered under a <code>static/</code> directory this is why we use <code>"staticDirPathInProjectBuildDirPath": "static"</code>. And finally we can see that everything we put in the <strong>public/</strong> directory is copied over to the <strong>build/</strong> directory when building so we use <code>"publicDirPath": "public"</code>.</p></figcaption></figure>

That's it, your project is ready to go!

You can run `npm run build-keycloak-theme`, the JAR distribution of your Keycloak theme will be generated in `build_keycloak` ([you can change this](../../build-options/keycloakifybuilddirpath.md)).

You're now able to use all the Keycloakify commands (`npx keycloakify --help`) from the root of your project.

{% hint style="success" %}
If you're currently using [keycloak-js](https://www.npmjs.com/package/keycloak-js) or [react-oidc-context](https://github.com/authts/react-oidc-context) to manage user authentication in your app you might want to checkout [oidc-spa](https://www.oidc-spa.dev/), the alternative from the Keycloakify team.

If you have any issues [reach out on Discord](https://discord.gg/mJdYJSdcm4)! We're here to help!
{% endhint %}

{% content-ref url="../../testing-your-theme/" %}
[testing-your-theme](../../testing-your-theme/)
{% endcontent-ref %}

{% content-ref url="../../customization-strategies/" %}
[customization-strategies](../../customization-strategies/)
{% endcontent-ref %}
