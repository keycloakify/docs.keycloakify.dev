# themeName

This is the name that will appear in the select input of the Keycloak Admin UI that let's you select the theme.

<figure><img src="../.gitbook/assets/image (62).png" alt="" width="375"><figcaption><p>Here the theme name is "my-react-app"</p></figcaption></figure>

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

The theme name is also in `kcContext.themeName`

Providing an array enables you to implement theme variant. See:

{% content-ref url="../theme-variants.md" %}
[theme-variants.md](../theme-variants.md)
{% endcontent-ref %}
