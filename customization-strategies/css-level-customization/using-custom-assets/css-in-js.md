# CSS-in-JS

{% tabs %}
{% tab title="Vite" %}
{% hint style="info" %}
TLDR: There is nothing specific to Keycloakify about importing assets. You can do it however you would in any other project.

Just if you're referencing assets that are in the public directory, use `import.meta.env.BASE_URL`
{% endhint %}
{% endtab %}

{% tab title="Webpack" %}
{% hint style="info" %}
TLDR:  You can import asset like you would in any other project, one exception being: If you reference assets that are located in your public directory from within your TSX files you must use Keycloakify's polifill of the `PUBLIC_URL` environnement variable, you can't use `process.env.PUBLIC_URL` directly:

```tsx
import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";
<img src={`${PUBLIC_URL}/my-image.png`} />
```
{% endhint %}
{% endtab %}
{% endtabs %}

CSS-in-JS is preferable over plain CSS as it enables for more flexibility and is easier to maintain.

Let's see, as an example, the different ways you have to change the background image of the login page.

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/**[**\<name of your theme>**](../../../build-options/themename.md)**/\<login|account>/resources/dist**

<img src="../../../.gitbook/assets/image (28).png" alt="" data-size="original">
{% endhint %}

Let's see how we can apply the image using a CSS-in-JS. In this example we'll use [@emotion/css](https://emotion.sh/docs/introduction).

```bash
yarn add @emotion/css
```

{% tabs %}
{% tab title="Vite" %}
<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import { css } from "@emotion/css";
</strong>import { Suspense, lazy } from "react";
// ...
export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;    
    // ...
    return (
        // ...
        &#x3C;DefaultPage
            kcContext={kcContext}
            classes={classes}
            // ...
        />
    );
}

const classes = {
<strong>    kcBodyClass: css({
</strong><strong>        "&#x26;&#x26;": { // Increase specificity so our rule takes precedence over the default background.
</strong><strong>            background: `url(${import.meta.env.BASE_URL}background.png) no-repeat center center fixed`,
</strong><strong>        }
</strong><strong>    })
</strong>} satisfies { [key in ClassKey]?: string };
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
<pre class="language-tsx" data-title="src/login/KcPages.tsx"><code class="lang-tsx"><strong>import { css } from "@emotion/css";
</strong><strong>import { PUBLIC_URL } from "keycloakify/PUBLIC_URL"; // You can't use process.env.PUBLIC_URL directly.
</strong>import { Suspense, lazy } from "react";
// ...
export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;    
    // ...
    return (
        // ...
        &#x3C;DefaultPage
            kcContext={kcContext}
            classes={classes}
            // ...
        />
    );
}

const classes = {
    kcBodyClass: css({
        "&#x26;&#x26;": { // Increase specificity so our rule takes precedence over the default background.
            background: `url(${PUBLIC_URL}/background.png) no-repeat center center fixed`,
        }
    })
} satisfies { [key in ClassKey]?: string };
</code></pre>
{% endtab %}
{% endtabs %}

Result (see [testing your theme](../../../testing-your-theme/)):

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Custom background successfully applied</p></figcaption></figure>

Now let's go a little further, it's even better to let the bundler generate url for your imports instead of manually referencing files from your public directory.\
So, let's move the background image in **src/login/assets/**:

<figure><img src="../../../.gitbook/assets/image (30).png" alt="" width="375"><figcaption></figcaption></figure>

And in our code import it this way:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import { css } from "@emotion/css";
<strong>import backgroundPngUrl from "./assets/background.png";
</strong>import { Suspense, lazy } from "react";
// ...
export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;    
    // ...
    return (
        // ...
        &#x3C;DefaultPage
            kcContext={kcContext}
            classes={classes}
            // ...
        />
    );
}

const classes = {
    kcBodyClass: css({
        "&#x26;&#x26;": {
<strong>            background: `url(${backgroundPngUrl}) no-repeat center center fixed`,
</strong>        }
    })
} satisfies { [key in ClassKey]?: string };
</code></pre>

Now let's see how we can go further and apply different background on different pages of our theme:

{% embed url="https://youtu.be/vRPlGUD-KvE" %}
