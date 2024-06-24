# 🖼️ Importing Assets

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

## JSX

Let's say you want to put te logo of your company on every pages of the theme. &#x20;

First you'd eject the Template:

```bash
npx keycloakify eject-page # Select login -> Template.tsx
```

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

This will create a src/login/Template.tsx file in your project.

### Import from the public directory

Let's use this plarceholder for the demo: [logo.png](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/logo.png).

We put the file in public/img/logo.png

<div align="center" data-full-width="false">

<figure><img src=".gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

Now let's edit the template to import the file: &#x20;

<pre class="language-tsx" data-title="src/login/Template.tsx"><code class="lang-tsx">export default function Template(props: TemplateProps&#x3C;KcContext, I18n>) {

    return (
        &#x3C;div className={kcClsx("kcLoginClass")}>
            &#x3C;div id="kc-header" className={kcClsx("kcHeaderClass")}>
                &#x3C;div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
<strong>                    {/*{msg("loginTitleHtml", realm.displayNameHtml)}*/}
</strong><strong>                    &#x3C;img src={`${import.meta.env.BASE_URL}img/logo.png`} width={500}/>
</strong>                &#x3C;/div>
            &#x3C;/div>
            {/* ... */}
</code></pre>

You can see the result by running `npx keycloakify start-keycloak`

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you ever need to SSH into the Keycloak server and hot swipe the image you can find it at

**/opt/keycloak/themes/\<name of your theme>/login/resources/dist/img/logo.png**

![](<.gitbook/assets/image (3).png>)
{% endhint %}

### Letting the bundle handle your import

Importing your asset from the public directory has the drawback that you won't get a compilation error if you made a mistake, like for example if you rename a file and forget to update the imports.  \
A nice solution for this is to let Vite or Webpack handle the import. &#x20;

Let's move our logo.png to **/src/login/assets/logo.png**

<figure><img src=".gitbook/assets/image (4).png" alt="" width="336"><figcaption></figcaption></figure>

Now let's update the imports:

<pre class="language-tsx" data-title="src/login/Template.tsx"><code class="lang-tsx">import logoPngUrl from "./assets/logo.png";

export default function Template(props: TemplateProps&#x3C;KcContext, I18n>) {

    return (
        &#x3C;div className={kcClsx("kcLoginClass")}>
            &#x3C;div id="kc-header" className={kcClsx("kcHeaderClass")}>
                &#x3C;div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
<strong>                    {/*{msg("loginTitleHtml", realm.displayNameHtml)}*/}
</strong><strong>                    &#x3C;img src={logoPngUrl} width={500}/>
</strong>                &#x3C;/div>
            &#x3C;/div>
            {/* ... */}
</code></pre>

This will yeild the same result except that now if you delete, move or rename the logo.png file you'll get a compilation error letting you know that you must also uptate your **Template.tsx** file. &#x20;

## Styles

Let's see, as an example, the different ways you have to change the backgrouns image of the login page. &#x20;

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/\<name of your theme>/\<login|account>/resources/dist**

![](<.gitbook/assets/image (28).png>)
{% endhint %}

### Plain CSS

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

### CSS-in-JS

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

That being said. It's even better to let the bundler generate url for your imports instead of manually referencing files from your public directory.  \
So, let's move the background image in **src/login/assets/**:

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

This will yeild the same result except that now if you delete, move or rename the **background.png** file you'll get a compilation error letting you know that you must also uptate your **KcPage.tsx** file.
