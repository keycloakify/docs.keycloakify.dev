# 🖋️ Custom Fonts

{% hint style="info" %}
TLDR: This is just a general purpose tutorial on how to import fonts in a web project.\
If you already know how to do it you can skip this page since there is nothing specific to Keycloakify about importing fonts. You can import them just as you would in any other web project.

Only, if you import the your fonts in the `index.html` don't forget to import them as well in `.storybook/preview-head.html` for Storybook.
{% endhint %}

## Using a web font service

Let's see how to use, for example, [Playwrite Netherland](https://fonts.google.com/specimen/Playwrite+NL) via Google Fonts.

First we need to add the few links tag we got from from Google Fonts in our HTML \<head>:

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

The fonts must also be imported in Storybook, so we add the links in the .storybook/preview-head.html as well:

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

Then all we have to do is apply the Font font family, in this example we will use vanilla CSS but you can of course use your favorite styling solution.

{% code title="src/login/main.css" %}
```css
.kcHeaderWrapperClass {
  /* NOTE: We would use `body {` if we'd like the font to be applied to everything. */
  font-family: "Playwrite NL", cursive;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

That's it!

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption><p>Playwrite NL successfully applied to the header</p></figcaption></figure>

## Using self hosted fonts

Keycloak is often used in enterprise internal network with strict network traffic control. In this context, using a Font CDN isn't an option, you want the font to be bundled in your jar and served directly by the Keycloak server.

Let's see how we would use a self hosted copy [Vercel's Geist](https://vercel.com/font) font.

First let's download and extract [the font files](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/geist.zip) in `src/login/assets/fonts/geist/`:

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

Now let's set Geist as the default font.

{% code title="src/login/main.css" %}
```css
@import url(./assets/fonts/geist/main.css);

body {
  font-family: Geist;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

Result:

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption><p>Geist successfully applied</p></figcaption></figure>
