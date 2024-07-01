# .css, .sass or .less

Let's see, as an example, the different ways you have to change the backgrouns image of the login page. &#x20;

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/**[**\<name of your theme>**](../../build-options/themename.md)**/\<login|account>/resources/dist**

![](<../../.gitbook/assets/image (28).png>)
{% endhint %}

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

Result (see [testing your theme](../../testing-your-theme/)):

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Custom background successfully applyed</p></figcaption></figure>

If you prefer, you can also move the background.png image from `public/` to, for examples, `src/login/assets/background.png` and reference the image with a path relative to the CSS file, in this case it would be:

{% code title="src/login/main.css" %}
```css
body.kcBodyClass {
    background: url(./assets/background.png) no-repeat center center fixed;
}
```
{% endcode %}

Now let's see how we would apply a different background image for different pages of our theme.

{% embed url="https://youtu.be/Kg7TqJhPvEI" %}
