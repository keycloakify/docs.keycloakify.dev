# themeVersion

Configure the version that will appear in the `pom.xml` file within the jar file of your theme.

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

By default the version that is used is the one in the package.json of your project

{% code title="package.json" %}
```json
{
  "version": "1.3.4"
}
```
{% endcode %}

But you can overwrite this value using an environment variable:

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      themeVersion: "1.2.3"
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
    "themeVersion": "1.2.3"
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

By default it's the package.json homepage field at reverse with .keycloak at the end.
