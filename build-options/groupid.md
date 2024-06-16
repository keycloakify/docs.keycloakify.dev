# groupId

Configure the `groupId` that will appear in the `pom.xml` file.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      groupId: "dev.keycloakify.demo-app-advanced.keycloak"
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
        "groupId": "dev.keycloakify.demo-app-advanced.keycloak"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

By default it's the package.json homepage field at reverse with .keycloak at the end.

You can overwrite this using an environment variable:

```bash
KEYCLOAKIFY_GROUP_ID="com.your-company.your-project.keycloak" npx keycloakify
```
