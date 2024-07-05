# In your Webpack Project

If you have a Wepack/React/TypeScript project you can integrate Keycloakify directly inside it. &#x20;

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

Next we want to repatriate the relevent files from [the starter template](https://github.com/keycloakify/keycloakify-starter) into our project:

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

Now you want to modify your entry point so that: &#x20;

* If the kcContext global is defined, render your Keycloakify theme
* Else, reder your App as usual. &#x20;

<pre class="language-tsx" data-title="src/index.tsx"><code class="lang-tsx">import { createRoot } from "react-dom/client";
import { StrictMode, lazy, Suspense } from "react";
/*
// The following block can be uncommented to test a specific page with `yarn dev`
// Don't forget to comment back or your bundle size will increase
<strong>import { getKcContextMock } from "./keycloak-theme/login/KcPageStory";
</strong>
if (process.env.NODE_ENV === "development") {
    window.kcContext = getKcContextMock({
        pageId: "register.ftl",
        overrides: {}
    });
}
*/

<strong>const KcLoginThemePage = lazy(() => import("./keycloak-theme/login/KcPage"));
</strong><strong>const KcAccountThemePage = lazy(() => import("./keycloak-theme/account/KcPage"));
</strong>const App = lazy(() => import("./App"));

createRoot(document.getElementById("root")!).render(
    &#x3C;StrictMode>
        &#x3C;Suspense>
            {(() => {
                switch (window.kcContext?.themeType) {
                    case "login":
                        return &#x3C;KcLoginThemePage kcContext={window.kcContext} />;
                    case "account":
                        return &#x3C;KcAccountThemePage kcContext={window.kcContext} />;
                }
<strong>                return &#x3C;App />;
</strong>            })()}
        &#x3C;/Suspense>
    &#x3C;/StrictMode>
);

declare global {
    interface Window {
        kcContext?:
<strong>            | import("./keycloak-theme/login/KcContext").KcContext
</strong><strong>            | import("./keycloak-theme/account/KcContext").KcContext;
</strong>    }
}
</code></pre>

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
</strong><strong>        "projectBuildDirPath": "build",
</strong><strong>        "staticDirPathInProjectBuildDirPath": "static",
</strong><strong>        "publicDirPath": "public"
</strong><strong>    },
</strong>    // ...
</code></pre>

Keycloakify has many build options that you can use, however `projectBuildDirPath`, `staticDirPathInProjectBuildDirPath` and `publicDirPath` are parameters specific to the use of Keycloakify in a Webpack context.

Theses **are not preferences!** If you're not using Create React App your Webpack configuration is probably different and you want to update thoses values to reflect how webpack build your site in your project. &#x20;

<figure><img src="../../.gitbook/assets/image (62).png" alt="" width="209"><figcaption><p>Here you can see that in a CRA project, when we run <code>npm run build</code> the app distribution is generated in a <strong>build/</strong> directory, this is why we use <code>"projectBuildDirPath": "build"</code>. We can also see that all the assets of the app are gathered under a <code>static/</code> directory this is why we use <code>"staticDirPathInProjectBuildDirPath": "static"</code>. And finally we can see that everything we put in the <strong>public/</strong> directory is copied over to the <strong>build/</strong> directory when building so we use <code>"publicDirPath": "public"</code>.</p></figcaption></figure>

That's it, your project is ready to go! &#x20;

You can run `npm run build-keycloak-theme`, the JAR distribution of your Keycloak theme will be generated in `dist_keycloak` ([you can change this](../../build-options/keycloakifybuilddirpath.md)).

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
