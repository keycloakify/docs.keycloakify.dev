# 🎭 Theme Variants





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
