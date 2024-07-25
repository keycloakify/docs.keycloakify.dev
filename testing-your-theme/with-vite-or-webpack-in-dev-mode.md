# With Vite or Webpack in dev mode

{% tabs %}
{% tab title="Vite" %}
{% hint style="info" %}
TLDR:

* Uncomment the `getKcContextMock()` in **src/main.tsx**
* **npm run dev**
* Don't forget to comment again when you're done testing.
{% endhint %}
{% endtab %}

{% tab title="Webpack" %}
{% hint style="info" %}
TLDR:

* Uncomment the `getKcContextMock()` in **src/index.tsx**
* **npm run start**
* Don't forget to comment again when you're done testing.
{% endhint %}
{% endtab %}
{% endtabs %}

If you don't have Storybook in your project you can also test your theme with the dev server.

To do that, just uncomment some line in your entrypoint:

{% tabs %}
{% tab title="Vite" %}
<pre class="language-tsx" data-title="src/main.tsx"><code class="lang-tsx">import { createRoot } from "react-dom/client";
import { StrictMode, lazy, Suspense } from "react";

<strong>import { getKcContextMock } from "./login/KcPageStory";
</strong>
<strong>if (import.meta.env.DEV) {
</strong><strong>    window.kcContext = getKcContextMock({
</strong><strong>        pageId: "register.ftl",
</strong><strong>        overrides: {}
</strong><strong>    });
</strong><strong>}
</strong>
const KcLoginThemePage = lazy(() => import("./login/KcPage"));
const KcAccountThemePage = lazy(() => import("./account/KcPage"));

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
                return &#x3C;h1>No Keycloak Context&#x3C;/h1>;
            })()}
        &#x3C;/Suspense>
    &#x3C;/StrictMode>
);
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
<pre class="language-tsx" data-title="src/index.tsx"><code class="lang-tsx">import { createRoot } from "react-dom/client";
import { StrictMode, lazy, Suspense } from "react";

<strong>import { getKcContextMock } from "./login/KcPageStory";
</strong>
<strong>if (process.env.NODE_ENV === "development") {
</strong><strong>    window.kcContext = getKcContextMock({
</strong><strong>        pageId: "register.ftl",
</strong><strong>        overrides: {}
</strong><strong>    });
</strong><strong>}
</strong>
const KcLoginThemePage = lazy(() => import("./login/KcPage"));
const KcAccountThemePage = lazy(() => import("./account/KcPage"));

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
                return &#x3C;h1>No Keycloak Context&#x3C;/h1>;
            })()}
        &#x3C;/Suspense>
    &#x3C;/StrictMode>
);
</code></pre>
{% endtab %}
{% endtabs %}

The pageId parameter of the getKcContextMock let you decide what page you want to test.\
The overrides parameter let you modify the the default kcContext mock for the page.\
\
For example you can set:

```tsx
window.kcContext = getKcContextMock({
  pageId: "login.ftl",
  overrides: {
    locale: {
      currentLanguageTag: "zh-CN",
    },
  },
});
```

For rendering the Login page in Chinese.

You can then run the development server with:

{% tabs %}
{% tab title="Vite" %}
```bash
npm run dev
```
{% endtab %}

{% tab title="Webpack (Create React App)" %}
```bash
npm run start
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
When you're done testing, don't forget to comment back the import of the mock. Forgetting to do so will negatively impact the bundle size of your pages.
{% endhint %}
