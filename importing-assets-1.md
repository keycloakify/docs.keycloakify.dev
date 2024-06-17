# 🖼️ Importing Assets

{% hint style="info" %}
TLDR: In Vite there isn't any Keycloakfy specifc restriction. You can import assets as you normaly would in a regular web project. &#x20;

In Webpack projects you can't use `process.env.PUBLIC_URL` directly you must use&#x20;

`import { PUBLIC_URL } from "keycloakify"` instead.&#x20;
{% endhint %}

Let's see, as an example, the different ways you have to change the backgrouns image of the login page. &#x20;

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/\<name of your theme>/\<login|account>/resources/dist**

![](<.gitbook/assets/image (28).png>)
{% endhint %}

## Plain CSS

Let's apply this image to the body using plain CSS

{% code title="src/login/main.css" %}
```css

body.kcBodyClass {
    background: url(/background.png) no-repeat center center fixed;
}
```
{% endcode %}

We import the StyleSheet:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

Result (see [testing your theme](testing-your-theme/)):

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption><p>Custom background successfully applyed</p></figcaption></figure>

## CSS-in-JS

Now let's see how we can apply the same image using a CSS-in-JS. In this example we'll use [@emotion/css](https://emotion.sh/docs/introduction).

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
</strong><strong>        "&#x26;&#x26;": { // Increace specificity so our rule takes precedence over the default background.
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
        "&#x26;&#x26;": { // Increace specificity so our rule takes precedence over the default background.
            background: `url(${PUBLIC_URL}/background.png) no-repeat center center fixed`,
        }
    })
} satisfies { [key in ClassKey]?: string };
</code></pre>
{% endtab %}
{% endtabs %}

And that's it. You'll get the same result as shown with plain CSS! &#x20;

That being said. It's even better to let the bundler handle the imports.  \
To do that, let's move the background image in src/login/assets/:

<figure><img src=".gitbook/assets/image (30).png" alt="" width="375"><figcaption></figcaption></figure>

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

Voilà!
