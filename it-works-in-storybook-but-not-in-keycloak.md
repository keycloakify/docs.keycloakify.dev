# It works in Storybook but not in Keycloak

If your images appear correctly in Storybook but are broken after deploying your theme to Keycloak—or when testing with `npx keycloakify start-keycloak`—the issue is likely due to how assets are imported.

\
:octagonal\_sign: **Incorrect Import Example**

The following approach is **not** valid for importing assets in Vite or Create React App (CRA), even outside of Keycloakify:

{% code title="Component.tsx" %}
```tsx
<img src="/logo.png" />
<img src="logo.png" />
```
{% endcode %}

While this may work in some cases, it is unreliable and not officially supported (By Vite or Webpack).

Below are two correct ways to import assets in TypeScript files when using Vite or Create React App (this is not specific to Keycloakify, it's how it should be done in any project):

## 1️⃣ Using the Bundler (Recommended)

Place your logo in `src/login/assets/logo.png`, then import it like this:

{% code title="src/login/Template.tsx" %}
```tsx
import logoPngUrl from "./assets/logo.png";

<img src={logoPngUrl} />
```
{% endcode %}

This method ensures that the bundler correctly resolves and includes the asset.

## 2️⃣ Using the Public Directory

If your logo is stored in `public/img/logo.png`, use one of the following approaches:

{% tabs %}
{% tab title="Vite" %}
{% code title="src/login/Template.tsx" %}
```tsx
<img src={import.meta.env.BASE_URL + "img/logo.png"} />
```
{% endcode %}
{% endtab %}

{% tab title="Create React App / Webpack" %}
{% code title="src/login/Template.tsx" %}
```tsx
import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";

<img src={PUBLIC_URL + "/img/logo.png"} />
```
{% endcode %}
{% endtab %}
{% endtabs %}

Using the public directory method is useful when assets should remain unchanged after the build process, but for most cases, bundling assets (Method 1) is the preferred approach.

***

By following these guidelines, your assets will load correctly in both Storybook and Keycloak.
