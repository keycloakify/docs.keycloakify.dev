# 🖼 Importing assets

If you're already familiar with the React ecosystem, you can import assets as you usually would; it should work out of the box.\
For those new to React or those interested in the specifics, here are some detailed instructions.

## Importing Custom Assets



{% tabs %}
{% tab title="Import managed by Webpack Asset Module" %}
Import using [webpack asset module](https://webpack.js.org/guides/asset-modules/) (available by default in Create React App) enable you to import assets directly in from your TypeScript files. This works as well in Keycloakify. &#x20;

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

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Keycloakify corectly replaces the URLs</p></figcaption></figure>
{% endtab %}

{% tab title="Import assets from the public/ directory" %}
This is a little less straight forward. Whenever possible prefer relying on the bundler for the import. &#x20;

Let's assume you have foo.png in the public directory. To import it you would do: &#x20;

{% code title="src/login/Template.tsx" %}
```
```
{% endcode %}
{% endtab %}
{% endtabs %}

To import your custom assets like images, videos, etc., place them in any folder under `src/`. Import them just like you would import code.

For example, you can place `foo.png` in `src/keycloak-theme/login/assets` and import it in the template as shown below:

```tsx
import fooPngUrl from "./assets/foo.png";
// NOTE: fooPngUrl is a URL (string) that points to the asset.

// ...

<img src={fooPngUrl} />
```

Note that this is not a unique feature of Keycloakify; Create React App, Vite, and Next.js all support importing assets in this manner.

There is one thing you can do in a vanilla Create React App project that you can't do with Keycloakify: placing assets in the `public/` directory and importing them manually.

For example, placing `foo.png` in `public/img/foo.png` and importing it won't work:

```tsx
// This works in CRA but not in Keycloakify
<img src={process.env.PUBLIC_URL + "img/foo.png"} />
// NOTE: process.env.PUBLIC_URL is usually the empty string
```

However, this method of asset importation is generally inferior in terms of both performance and maintainability, so its lack of support shouldn't be a concern.

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
