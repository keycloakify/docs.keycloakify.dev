# 🎭 Theme Variants

This is the name of the theme in the Keycloak admin select:

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

By default it's `package.json["name"]`

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      themeName: "my-custom-name"
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
    "keycloakify": {
        "themeName": "my-custom-name"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

You can also provide an array if you want to Keycloakify to create multiple theme variant:

{% tabs %}
{% tab title="Vite" %}
{% code title="vite.config.ts" %}
```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
      themeName: [ "keycloakify-starter", "keycloakify-starter-variant-1" ]
    })
  ],
})
```
{% endcode %}
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
    "keycloakify": {
        "themeName": [ "keycloakify-starter", "keycloakify-starter-variant-1" ]
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

This option deprecates `extraThemeNames`and let you pack multiple themes variant in a single `.jar` bundle. In vanilla Keycloak themes you have the ability to extend a base theme. There is now an idiomatic way of achieving the same result by using this option.

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

The theme name will be available on the `kcContext`:

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

You'll be able to implement different behaviour based on which theme variant is the current one:

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

To load different global css file based on the theme name you can implement this strategy:

```typescript
// src/keycloak-theme/login/useGlobalStylesheet.ts
import { useMemo } from "react";

export function useGlobalStylesheet(themeName: string){
    useMemo(() => {
        switch(themeName){
            case "keycloakify-starter":
                // @ts-expect-error
                import("./keycloakify-starter.css");
                break;
            case "keycloakify-starter-variant-1":
                // @ts-expect-error
                import("./keycloakify-starter-variant-1.css");
                break;
        }
    }, [themeName]);
}
```

```tsx
// src/keycloak-theme/login/KcApp.tsx

import { useGlobalStylesheet } from "./useGlobalStylesheet";
// ...

export default function KcApp(props: { kcContext: KcContext; }) {
  const { kcContext }= props;
  // ...
  
  useGlogalStylesheet(kcContext.themeName);

  // ...
```
