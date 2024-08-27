# In your Vite Project

If you have a Vite/React/TypeScript project you can integrate Keycloakify directly inside it. &#x20;

&#x20;In this guide we're going to work with a vanilla Vite project.

<figure><img src="../../.gitbook/assets/image (56).png" alt="" width="375"><figcaption><p>Creating a new vite project with yarn create vite. You don't need to create a new project. Just use your existing codebase.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (57).png" alt="" width="368"><figcaption><p>Our codebase before installing Keycloakify</p></figcaption></figure>

{% hint style="info" %}
Before anything make sure to commit all your pending changes so you can easily revert changes if need be.
{% endhint %}

Let's start by installing Keycloakify (and optionally Storybook) to our project:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add keycloakify
yarn add --dev storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add keycloakify
pnpm add --dev storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="bun" %}
```bash
bun add keycloakify
bun add --dev storybook @storybook/react @storybook/react-vite
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install --save keycloakify
npm install --save-dev storybook @storybook/react @storybook/react-vite
```
{% endtab %}
{% endtabs %}

Next we want to repatriate the relevent files from [the starter template](https://github.com/keycloakify/keycloakify-starter) into our project:

```bash
cd my-react-app
git clone https://github.com/keycloakify/keycloakify-starter tmp
mv tmp/src src/keycloak-theme
mv tmp/.storybook .
rm -rf tmp
rm src/keycloak-theme/vite-env.d.ts
mv src/keycloak-theme/main.tsx src/main.tsx
```

<figure><img src="../../.gitbook/assets/after_import.png" alt="" width="370"><figcaption><p>State of your codebase after bringin in the Keycloakify boilerplate code.<br>Note thate the keycloak-theme (or keycloak_theme) directory can be located anywhere under your src directory.</p></figcaption></figure>

Now you want to modify your entry point so that: &#x20;

* If the kcContext global is defined, render your Keycloakify theme
* Else, reder your App as usual. &#x20;

<pre class="language-tsx" data-title="src/main.tsx"><code class="lang-tsx">/* eslint-disable react-refresh/only-export-components */
import { createRoot } from "react-dom/client";
import { 
    StrictMode,
<strong>    lazy,
</strong><strong>    Suspense
</strong>} from "react";
<strong>import { KcPage, type KcContext } from "./keycloak-theme/kc.gen";
</strong><strong>const App = lazy(() => import("./App"));
</strong>
<strong>// The following block can be uncommented to test a specific page with `yarn dev`
</strong><strong>// Don't forget to comment back or your bundle size will increase
</strong><strong>/*
</strong><strong>import { getKcContextMock } from "./keycloak-theme/login/KcPageStory";
</strong><strong>
</strong><strong>if (import.meta.env.DEV) {
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
#### Question:

Why do my main application and Keycloak theme share the same entry point?

#### Answer:

To simplify the build process. If you don't want it to negatively impact the performance of your application, it's essential to understand the following points:

* **Different Contexts:** The application (`App`) and Keycloak page (`KcPage`) are mounted in very different contexts. Avoid sharing providers between the two at the `main.tsx` file level. The true entry point of your application is the `App` component, while the entry point for your Keycloak theme is the `KcPage` component. Be careful about what code is shared between them.
* **Responsibility of main.tsx:** The `main.tsx` file should only determine the context (either the application or Keycloak) and mount the appropriate component (`App` or `KcPage`). It should not contain any substantial logic or dependencies.
* **Performance Considerations:** Keep `main.tsx` as lightweight as possible to avoid increasing the initial load time of both your main application and login pages. For example, do not load any state management libraries like `redux-toolkit` at this level.&#x20;
{% endhint %}

You also need to use Keycloakify's Vite plugin. Here we don't provide any [build options](../../configuration-options/) but you probably at least want to define [keycloakVersionTargets](../../configuration-options/keycloakversiontargets.md).

<pre class="language-tsx" data-title="vite.config.ts"><code class="lang-tsx">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
<strong>import { keycloakify } from "keycloakify/vite-plugin";
</strong>
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(), 
<strong>    keycloakify({
</strong><strong>        accountThemeImplementation: "none"
</strong><strong>    })
</strong>  ],
})
</code></pre>

{% hint style="info" %}
Leave accountThemeImplementation set to "none" for now.  \
To initialize the account theme refer to [this guide](broken-reference).&#x20;
{% endhint %}

Finally you want to add to your package.json a script for building the theme and another one to start storybook.

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "my-react-app",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc -b &#x26;&#x26; vite build",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
<strong>    "build-keycloak-theme": "npm run build &#x26;&#x26; keycloakify build",
</strong><strong>    "storybook": "storybook dev -p 6006"
</strong>  },
  // ...
</code></pre>

That's it, your project is ready to go! &#x20;

You can run npm run build-keycloak-theme, the JAR distribution of your Keycloak theme will be generated in dist\_keycloak.

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
