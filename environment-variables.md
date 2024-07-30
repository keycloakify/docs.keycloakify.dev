# 🔧 Accessing the Server Environment Variables

Environment variables defined on the Keycloak server can be transferred to the theme. This allows for a degree of theme customization without necessitating a rebuild. This approach is particularly useful if multiple parties are reusing your theme. As an example, you can distribute a single .jar file to multiple customers, enabling them to modify certain aspect of the login page by defining specific environment variables.

Let's define two environnement variable: &#x20;

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
            // ...
<strong>            environmentVariables: [
</strong><strong>                { name: "MY_APP_API_URL", default: "" },
</strong><strong>                { name: "MY_APP_PALETTE", default: "dracula" }
</strong><strong>            ]
</strong>        })
    ]
});

</code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
    "keycloakify": {
        "environmentVariables": [
            { "name": "MY_APP_API_URL", "default": "" },
            { "name": "MY_APP_PALETTE", "default": "dracula" }
        ]
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

We can then access the runtime value of thoses variables under kcContext.properties:

<figure><img src=".gitbook/assets/image (65).png" alt=""><figcaption><p>Accessing the value of the environement variable defined.</p></figcaption></figure>

Now let's see how you can set the value of thoses environement variable on the Keycloak side:

{% tabs %}
{% tab title="Docker" %}
<pre class="language-bash"><code class="lang-bash">docker run \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
<strong>    --env MY_APP_API_URL='https://api.my-org.com' \
</strong><strong>    --env MY_APP_PALETTE='solaris'
</strong>    -p 8080:8080 \
    docker-keycloak-with-theme
</code></pre>
{% endtab %}

{% tab title="Helm" %}
{% code title="values.json" %}
```bash
keycloak:
  initContainers: |
    - name: realm-ext-provider
      image: curlimages/curl
      imagePullPolicy: IfNotPresent
      command:
        - sh
      args:
        - -c
        - |
          # Replace USER and PROJECT.    
          curl -L -f -S -o /extensions/keycloak-theme.jar https://github.com/USER/PROJECT/releases/latest/download/keycloak-theme-for-kc-24.jar

      volumeMounts:
        - name: extensions
          mountPath: /extensions

  extraVolumeMounts: |
    - name: extensions
      mountPath: /opt/bitnami/keycloak/providers

  extraVolumes: |
    - name: extensions
      emptyDir: {}
      
  extraEnv: |
    - name: MY_APP_API_URL
      value: 'https://api.my-org.com'
    - name: MY_APP_PALETTE
      value: 'solaris'
```
{% endcode %}
{% endtab %}

{% tab title="Bare Metal" %}
```bash
MY_APP_API_URL="https://api.my-org.com" MY_APP_PALETTE="solaris" /opt/keycloak/bin/kc.sh start
```
{% endtab %}
{% endtabs %}

