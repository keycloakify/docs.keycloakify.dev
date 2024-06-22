# 🖋️ Custom Fonts

{% hint style="info" %}
TLDR: This is just a general purpose tutorial on how to import fonts in a web project.\
If you already know how to do it you can skip this page since there is nothing specific to Keycloakify about importing fonts. You can import them just as you would in any other web project. &#x20;

Only, if you import the your fonts in the `index.html` don't forget to import them as well in `.storybook/preview-head.html` for Storybook.
{% endhint %}

## Using a web font service

Let's see how to use, for example, [Playwrite Netherland](https://fonts.google.com/specimen/Playwrite+NL) via Google Fonts. &#x20;

First we need to add the few links tag we got from from Google Fonts in our HTML \<head>: &#x20;

<pre class="language-html" data-title="index.html (or public/index.html in Webpack)"><code class="lang-html">&#x3C;!doctype html>
&#x3C;html>
    &#x3C;head>
        &#x3C;meta charset="utf-8" />
        &#x3C;meta name="viewport" content="width=device-width, initial-scale=1" />

        &#x3C;link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />

<strong>        &#x3C;link rel="preconnect" href="https://fonts.googleapis.com">
</strong><strong>        &#x3C;link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
</strong><strong>        &#x3C;link href="https://fonts.googleapis.com/css2?family=Playwrite+NL:wght@100..400&#x26;display=swap" rel="stylesheet">
</strong>
    &#x3C;/head>

    &#x3C;body>
        &#x3C;div id="root">&#x3C;/div>
        &#x3C;script type="module" src="/src/main.tsx">&#x3C;/script>
    &#x3C;/body>
&#x3C;/html>
</code></pre>

The fonts must also be imported in Storybook, so we add the links in the .storybook/preview-head.html as well: &#x20;

<pre class="language-html" data-title=".storybook/preview-head.html"><code class="lang-html">&#x3C;style>
    body.sb-show-main.sb-main-padded {
        padding: 0;
    }
    
    /* ... */
&#x3C;/style>

<strong>&#x3C;link rel="preconnect" href="https://fonts.googleapis.com">
</strong><strong>&#x3C;link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
</strong><strong>&#x3C;link href="https://fonts.googleapis.com/css2?family=Playwrite+NL:wght@100..400&#x26;display=swap" rel="stylesheet">
</strong></code></pre>

Then all we have to do is apply the Font font familly, in this example we will use vanilla CSS but you can of course use your favourite styling solution.

{% code title="src/login/main.css" %}
```css
.kcHeaderWrapperClass { /* NOTE: We would use `body {` if we'd like the font to be applied to everything. */
    font-family: "Playwrite NL", cursive;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

That's it!

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption><p>Playwrite NL succesfully applied to the header</p></figcaption></figure>

## Using self hosted fonts

Keycloak is often used in entreprise internal network with strict network trafic control. In this context, using a Font CDN isn't an option, you want the font to be bundled in your jar and served directly by the Keycloak server. &#x20;

Let's see how we would use a self hosted copy [Vercel's Geist](https://vercel.com/font) font.

First let's download and extract [the font files](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/geist.zip) in `src/login/assets/fonts/geist/`:

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

Now let's import the font:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./assets/fonts/geist/main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

Now the Geist is available to use in the project but we still need to actually apply it to some element.\
Let's set it as the default font for everything. &#x20;

{% code title="src/login/main.css" %}
```css
body {
    font-family: Geist;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import "./assets/fonts/geist/main.css";
<strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

Result:&#x20;

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption><p>Geist succesfully applyed</p></figcaption></figure>
