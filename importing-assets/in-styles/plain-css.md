# Plain CSS

{% hint style="info" %}
At this point we assume we have a **background.png** file in the **public/** directory.
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

