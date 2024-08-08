# Using custom assets

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

Let's say you want to put te logo of your company on every pages of the theme.

First you'd eject the Template:

```bash
npx keycloakify eject-page # Select login -> Template.tsx
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This will create a src/login/Template.tsx file in your project.

## Import from the public directory

Let's use this placeholder for the demo: [logo.png](https://github.com/keycloakify/keycloakify/releases/download/v0.0.1/logo.png).

We put the file in public/img/logo.png

<div align="center" data-full-width="false">

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

Now let's edit the template to import the file:

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

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you ever need to SSH into the Keycloak server and hot swipe the image you can find it at

**/opt/keycloak/themes/**[**\<name of your theme>**](../../build-options/themename.md)**/login/resources/dist/img/logo.png**

<img src="../../.gitbook/assets/image (3) (1).png" alt="" data-size="original">
{% endhint %}

## Letting the bundle handle your import

Importing your asset from the public directory has the drawback that you won't get a compilation error if you made a mistake, like for example if you rename a file and forget to update the imports.\
A nice solution for this is to let Vite or Webpack handle the import.

Let's move our logo.png to **/src/login/assets/logo.png**

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt="" width="336"><figcaption></figcaption></figure>

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

This will yield the same result except that now if you delete, move or rename the logo.png file you'll get a compilation error letting you know that you must also update your **Template.tsx** file.
