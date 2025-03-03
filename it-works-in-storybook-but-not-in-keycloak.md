# It works in Storybook but not in Keycloak

If you are facing issues with broken images links after you deploy your theme to keycloak or testing with npx keycloakify start-keycloak the issue is related with the way you import your assets.  \
\
:octagonal\_sign: Incorect import example:  \


{% code title="Component.tsx" %}
```tsx
<img src="/logo.png"/>
<img src="logo.png"/>
```
{% endcode %}

This is not a proper way to import assets in Vite or Create-React-App, even outside of keycloakify.

It happen to work by coincidence in most cases but regardless, this is not supported.

Here are the two proper way to import an asset in your TypeScript files in Vite or Create-React-App:

## Using the bundler (recommended)

Put your logo in in src/login/assets/logo.png\
\
Import your logo like this:

{% code title="src/login/Template.tsx" %}
```tsx
import logoPngUrl from "./assets/logo.png";

<img src={logoPngUrl} />
```
{% endcode %}

## From the public directory

Assuming your logo is in public/img/logo.png

{% tabs %}
{% tab title="Vite" %}
{% code title="src/login/Template.tsx" %}
```tsx
<img src={import.meta.env.BASE_URL + "img/logo.png"} />
```
{% endcode %}
{% endtab %}

{% tab title="Create-React-App/Webpack" %}
{% code title="src/login/Template.tsx" %}
```tsx
import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";

<img src={PUBLIC_URL + "/img/logo.png"
```
{% endcode %}
{% endtab %}
{% endtabs %}
