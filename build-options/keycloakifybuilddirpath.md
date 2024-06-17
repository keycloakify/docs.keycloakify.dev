# keycloakifyBuildDirPath

This option enables you to configure in which directory the .jar files should be created. By default it's `dist_keycloak` in Vite or `build_keycloak` in Webpack. &#x20;

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      keycloakifyBuildDirPath: "./keycloak-theme-dist"
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
        "keycloakifyBuildDirPath": "./keycloak-theme-jars"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
