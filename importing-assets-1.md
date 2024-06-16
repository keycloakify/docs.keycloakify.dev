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
YIf you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/\<name of your theme>/\<login|account>/resources/dist**

<img src=".gitbook/assets/image (27).png" alt="" data-size="original">
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

Now let's see how we can apply the same image using a CSS-in-JS
