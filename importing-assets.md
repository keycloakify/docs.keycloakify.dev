# 🖋️ Custom Fonts

## Using web font services

Web font services such as Google Fonts will work without any Keycloakify specific setup.  \
Let's see how to use, for example [Playwrite Netherland](https://fonts.google.com/specimen/Playwrite+NL) via Google Fonts. &#x20;

<pre class="language-html" data-title="index.html"><code class="lang-html">&#x3C;!doctype html>
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

The fonts must also be imported in Storybook (.storybook/preview-head.html): &#x20;

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
.kcHeaderWrapperClass {
    /* Google font */
    font-family: "Playwrite NL", cursive;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

That's it!

<figure><img src=".gitbook/assets/image (1).png" alt="" width="375"><figcaption><p>Playwrite NL succesfully applied to the header</p></figcaption></figure>

## Using self hosted fonts

Keycloak is often used in entreprise internal network with strict network trafic controle. In this case using a Font CDN isn't an option, you want the font to be bundled in your jar and served directly by the Keycloak server. &#x20;

In regular web application you would import the font in your html with something like: `<link rel="stylesheet" href="/fonts/my-font.css">` or in your TypeScript code like `import "@fontsource/roboto/400.css"`.  \
Unfortunatly, for technical reasons, theses approach won't wonk in Keycloakify, the `@font-face` imports must be done inline, in the `index.html`.  \


Let's see how we would use a self hosted copy [Vercel's Geist](https://vercel.com/font) font in a Keycloakify project.

First let's download and extract [the font files](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/geist.zip) in `public/fonts/geist/`:

<figure><img src=".gitbook/assets/image (2).png" alt="" width="265"><figcaption></figcaption></figure>

Then we need to add an inline `<style />` tag in the `index.html`

<pre class="language-html" data-title="index.html"><code class="lang-html">&#x3C;!doctype html>
&#x3C;html>
    &#x3C;head>
        &#x3C;meta charset="utf-8" />
        &#x3C;meta name="viewport" content="width=device-width, initial-scale=1" />

        &#x3C;link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />

<strong>        &#x3C;style>
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Black.woff2') format('woff2');
</strong><strong>                font-weight: 900;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Bold.woff2') format('woff2');
</strong><strong>                font-weight: bold;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Light.woff2') format('woff2');
</strong><strong>                font-weight: 300;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Medium.woff2') format('woff2');
</strong><strong>                font-weight: 500;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Regular.woff2') format('woff2');
</strong><strong>                font-weight: 400;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-SemiBold.woff2') format('woff2');
</strong><strong>                font-weight: 600;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-Thin.woff2') format('woff2');
</strong><strong>                font-weight: 100;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-UltraLight.woff2') format('woff2');
</strong><strong>                font-weight: 200;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist';
</strong><strong>                src: url('/fonts/geist/Geist-UltraBlack.woff2') format('woff2');
</strong><strong>                font-weight: 950;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>            @font-face {
</strong><strong>                font-family: 'Geist Variable';
</strong><strong>                src: url('/fonts/geist/Geist-Variable.woff2') format('woff2');
</strong><strong>                font-weight: 100 950;
</strong><strong>                font-style: normal;
</strong><strong>            }
</strong><strong>        &#x3C;/style>
</strong>
    &#x3C;/head>

    &#x3C;body>
        &#x3C;div id="root">&#x3C;/div>
        &#x3C;script type="module" src="/src/main.tsx">&#x3C;/script>
    &#x3C;/body>
&#x3C;/html>

</code></pre>

{% hint style="info" %}
In Webpack project prefer using `%PUBLIC_URL%/fonts/geist/...` in the `public/index.html`.
{% endhint %}

The fonts must also be imported in Storybook (`.storybook/preview-head.html`): &#x20;

<pre class="language-html" data-title=".storybook/preview-head.html"><code class="lang-html">&#x3C;style>
    body.sb-show-main.sb-main-padded {
        padding: 0;
    }
    
    /* ... */
    
<strong>    @font-face {
</strong><strong>        font-family: 'Geist';
</strong><strong>        src: url('/fonts/geist/Geist-Black.woff2') format('woff2');
</strong><strong>        font-weight: 900;
</strong><strong>        font-style: normal;
</strong><strong>    }
</strong><strong>    /* ...and all the other @font-face */
</strong>
&#x3C;/style>
</code></pre>

Then all we have to do is apply the Font font familly, in this example we will use vanilla CSS but you can of course use your favourite styling solution.

{% code title="src/login/main.css" %}
```css
body {
    font-family: Geist;
}
```
{% endcode %}

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>import { Suspense, lazy } from "react";
// ...
</code></pre>

Result:&#x20;

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption><p>Geist succesfully applyed</p></figcaption></figure>
