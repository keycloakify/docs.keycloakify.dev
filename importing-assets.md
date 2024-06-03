# 🖼️ Importing assets and fonts

## Importing Custom assets that aren't fonts

{% tabs %}
{% tab title="Using the `import` statement " %}
For example, you can place `foo.png` in `src/login/assets` and import it in the template as shown below:

{% code title="src/login/Template.tsx" lineNumbers="true" %}
```tsx
import fooPngUrl from "./assets/foo.png";
// NOTE: fooPngUrl is a URL (string) that points to the asset.

// ...

<img src={fooPngUrl} />
```
{% endcode %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/c3e706746ac2b347d85bcae72bdfa394ccb6d1b0" %}
Demo in the Starter project
{% endembed %}

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Keycloakify correctly replaces the URLs</p></figcaption></figure>
{% endtab %}

{% tab title="from public/ - Vite" %}
This is how you would reliably import assets that you have in your public directory regardless of if you are in a Keycloak context or not.

Let's assume you have `foo.png in` the `public/` directory. To import it you would do.

#### In you `public/index.html` file

<pre class="language-html" data-title="public/index.html"><code class="lang-html">&#x3C;!DOCTYPE html>
&#x3C;html>
&#x3C;head>
<strong>  &#x3C;link rel="icon" href="/foo.png" />  
</strong>&#x3C;/head>

&#x3C;body>
  &#x3C;div id="root">&#x3C;/div>
&#x3C;/body>

&#x3C;/html>
</code></pre>

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>The favicon is correctly loaded for the Keycloak server</p></figcaption></figure>

#### In your TypeScript files

{% hint style="warning" %}
This is **not recommended**, Keycloakify or not, whenever possible prefer importing your assets using the `import` statement. [Learn more](https://create-react-app.dev/docs/using-the-public-folder/#adding-assets-outside-of-the-module-system).
{% endhint %}

{% code title="src/login/Template.tsx" %}
```tsx
// Note: Base url is often just "/" but please use the BASE_URL env
// as it enable Keycloakify to resolve your assets when in Keycloak.   
<img src={`${import.meta.env.BASE_URL}foo.png`} />
```
{% endcode %}

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>The asset is successfully downloaded</p></figcaption></figure>
{% endtab %}

{% tab title="from public/ - CRA (Webpack)" %}
This is how you would reliably import assets that you have in your public directory regardless of if you are in a Keycloak context or not.

Let's assume you have `foo.png in` the `public/` directory. To import it you would do.

#### In you `public/index.html` file

<pre class="language-html" data-title="public/index.html"><code class="lang-html">&#x3C;!DOCTYPE html>
&#x3C;html>
&#x3C;head>
<strong>  &#x3C;link rel="icon" href="%PUBLIC_URL%/foo.png" />  
</strong>&#x3C;/head>

&#x3C;body>
  &#x3C;div id="root">&#x3C;/div>
&#x3C;/body>

&#x3C;/html>
</code></pre>

{% embed url="https://github.com/keycloakify/keycloakify-starter/blob/d28c986686c3255b812152f68f3458995fa8bfb7/public/index.html#L8-L17" %}
Example from the starter: Import favicon
{% endembed %}

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>The favicon is correctly loaded for the Keycloak server</p></figcaption></figure>

#### In your TypeScript files

{% hint style="warning" %}
This is **not recommended**, Keycloakify or not, whenever possible prefer importing your assets using the `import` statement. [Learn more](https://create-react-app.dev/docs/using-the-public-folder/#adding-assets-outside-of-the-module-system).
{% endhint %}

{% code title="src/login/Template.tsx" %}
```tsx
// This is like process.env.PUBLIC_URL but it works both inside
// and outside of Keycloak.  
import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";

<img src={`${PUBLIC_URL}/foo.png`} />
```
{% endcode %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/7a3589610cb27663d0e5d2ac68bb3d4b1c7c9970" %}
Example in the starter
{% endembed %}

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>The asset is successfully downloaded</p></figcaption></figure>
{% endtab %}
{% endtabs %}

## Importing Fonts

{% tabs %}
{% tab title="Self hosted fonts - Vite" %}
So, this is typically how you would import self hosted font in a Regular React App.\
Let's imagine you have the following font index CSS file:

{% code title="public/fonts.css" %}
```css
@font-face {
  font-family: Marianne;
  src: url("./fonts/Marianne-Light.woff2") format("woff2");
  font-weight: 300;
  font-style: normal;
  font-display: swap;
}
```
{% endcode %}

You would import it like this:

{% code title="public/index.html" %}
```html
<!-- 🛑 This do not work in keycloakfy --> 
<link rel="stylesheet" href="%BASE_URL%font.css" />
```
{% endcode %}

Unfortunately this approach does not work in Keycloakify.

The workaround consist in including all your `@font-face` statements directly in your `public/index.html` file.\
This is how you would update your index.html file in order to make it work with Keycloakify:

{% code title="public/index.html" %}
```diff
-<link rel="stylesheet" href="%BASE_URL%font.css" />
+<style>
+ @font-face {
+   font-family: Marianne;
+   src: url("%BASE_URL%fonts/Marianne-Light.woff2") format("woff2");
+   font-weight: 300;
+   font-style: normal;
+   font-display: swap;
+ }
+</style>
```
{% endcode %}

Example [here](https://github.com/garronej/keycloakify-demo-app/blob/9aa2dbaec28a7786d6b2983c9a59d393dec1b2d6/public/index.html#L27-L73) (and the font are [here](https://github.com/garronej/keycloakify-demo-app/tree/main/public/fonts/WorkSans)).

{% hint style="info" %}
**Storybook**: To have your font applied in your Storybook as well you need to import them as shown [here](https://github.com/keycloakify/keycloakify-starter/blob/20e45b981f4db907139ae8e3111be34f83abc9b6/src/keycloak-theme/login/createPageStory.tsx#L20-L21).
{% endhint %}
{% endtab %}

{% tab title="Self hosted fonts - Webpack" %}
So, this is typically how you would import self hosted font in a Regular React App.\
Let's imagine you have the following font index CSS file:

{% code title="public/fonts.css" %}
```css
@font-face {
  font-family: Marianne;
  src: url("./fonts/Marianne-Light.woff2") format("woff2");
  font-weight: 300;
  font-style: normal;
  font-display: swap;
}
```
{% endcode %}

You would import it like this:

{% code title="public/index.html" %}
```html
<!-- 🛑 This do not work in keycloakfy --> 
<link rel="stylesheet" href="%PUBLIC_URL%/font.css" />
```
{% endcode %}

Unfortunately this approach does not work in Keycloakify.

The workaround consist in including all your `@font-face` statements directly in your `public/index.html` file.\
This is how you would update your index.html file in order to make it work with Keycloakify:

{% code title="public/index.html" %}
```diff
-<link rel="stylesheet" href="%PUBLIC_URL%/font.css" />
+<style>
+ @font-face {
+   font-family: Marianne;
+   src: url("%PUBLIC_URL%/fonts/Marianne-Light.woff2") format("woff2");
+   font-weight: 300;
+   font-style: normal;
+   font-display: swap;
+ }
+</style>
```
{% endcode %}

Example [here](https://github.com/garronej/keycloakify-demo-app/blob/9aa2dbaec28a7786d6b2983c9a59d393dec1b2d6/public/index.html#L27-L73) (and the font are [here](https://github.com/garronej/keycloakify-demo-app/tree/main/public/fonts/WorkSans)).

{% hint style="info" %}
**Storybook**: To have your font applied in your Storybook as well you need to import them as shown [here](https://github.com/keycloakify/keycloakify-starter/blob/20e45b981f4db907139ae8e3111be34f83abc9b6/src/keycloak-theme/login/createPageStory.tsx#L20-L21).
{% endhint %}
{% endtab %}

{% tab title="Using Google Font or other font provider" %}
Works out of the box ✅
{% endtab %}
{% endtabs %}

## Importing Default Keycloak Theme Resources

{% hint style="info" %}
This section is mainly for transparency.\
You should use your own assets instead of importing the default ones; that's the whole point of creating a custom theme.
{% endhint %}

To import resources available in the default theme, you can construct URLs like:

```tsx
const patternflyUrl = `${kcContext.url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly.min.css`;
const loginCssUrl = `${url.resourcesPath}/css/login.css`;
```

You can see what assets are available under `public/keycloak-resources/login/resources`.\
If you want to choose which version of the assets to use, refer to [this build option](build-options.md#loginthemeresourcesfromkeycloakversion).

By default, the default CSS assets are imported and applied [here](https://github.com/keycloakify/keycloakify/blob/402c6fc64a26268b6f2f7222e4f11ff07de452f8/src/login/Template.tsx#L35-L38C19) in the login theme.
