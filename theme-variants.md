# 🎭 Theme Variants

Theme variant enables you to create multiples Keycloak theme with a single codebase. &#x20;

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

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

In your code you'll be able to load different styles basesd on the value of `kcContext.themeName`:

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

