# 🎭 Theme Variants

Theme variant enables you to create multiples Keycloak theme with a single codebase.

{% tabs %}
{% tab title="Vite" %}
{% code title="vite.config.ts" %}
```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(),
    keycloakify({
      themeName: ["keycloakify-starter", "keycloakify-starter-variant-1"],
    }),
  ],
});
```
{% endcode %}
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
  "keycloakify": {
    "themeName": ["keycloakify-starter", "keycloakify-starter-variant-1"]
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

In your code you'll be able to load different styles based on the value of `kcContext.themeName`:

<figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption><p>NOTE: You need to <code>run npm run dev</code>, <code>npm run storybook</code> or <code>npm run build-keycloak-theme</code> for the types to be updated.</p></figcaption></figure>

{% embed url="https://youtu.be/Nkoz1iD-HOA" %}
Tutorial video
{% endembed %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/theme_variant" %}
Branch of the starter where the changes of the video have been applied
{% endembed %}
