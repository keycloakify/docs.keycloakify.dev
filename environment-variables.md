# 🔧 Environment Variables

Environment variables defined on the Keycloak server can be transferred to the theme. This allows for a degree of theme customization without necessitating a rebuild. This approach is particularly useful if multiple parties are reusing your theme. As an example, you can distribute a single .jar file to multiple customers, enabling them to modify certain aspect of the login page by defining specific environment variables. &#x20;

Concretely the ide is to be able, if you start your Keycloak server like this: &#x20;

<pre class="language-bash"><code class="lang-bash">docker run \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
<strong>    --env MY_ENV_VARIABLE='Value of my env variable' \
</strong>    -p 8080:8080 \
    docker-keycloak-with-theme
</code></pre>

To be able to access MY\_ENV\_VARIABLE value in your theme. (This is assuming you're using docker but for for Helm for example see [this](importing-your-theme-in-keycloak.md#using-helm)).

To implement this you need to use [the extraThemeProperties build option](build-options.md#extrathemeproperties) like so:

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      extraThemeProperties: [ 
</strong><strong>        "MY_ENV_VARIABLE=${env.MY_ENV_VARIABLE:default}"
</strong><strong>      ]
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
<strong>        "extraThemeProperties": [ 
</strong><strong>            "MY_ENV_VARIABLE=${env.MY_ENV_VARIABLE:default}"
</strong><strong>        ]
</strong>    }
}
</code></pre>
{% endtab %}
{% endtabs %}

Then you will be able to access the value of MY\_ENV\_VARIABLE with kcContext.properties.MY\_ENV\_VARIABLE. Example: &#x20;

{% code title="login/Template.tsx" %}
```tsx
import { useEffect } from "react";
import { type TemplateProps } from "keycloakify/login/TemplateProps";
import type { KcContext } from "./kcContext";
import type { I18n } from "./i18n";

export default function Template(props: TemplateProps<KcContext, I18n>) {
    const { kcContext } = props;

    useEffect(() => {
        console.log(kcContext.properties.MY_ENV_VARIABLE);
    }, []);

    //...

}
```
{% endcode %}

You also probably want to provide mock values the variables for when you're developing your theme in Storybook: &#x20;

<pre class="language-typescript" data-title="login/kcContext.ts"><code class="lang-typescript">import { createGetKcContext } from "keycloakify/login";

export const { getKcContext } = createGetKcContext({
  mockData: [ /* ... */],
<strong>  mockProperties: {
</strong><strong>    MY_ENV_VARIABLE: "Mocked value"
</strong><strong>  }
</strong>});

export const { kcContext } = getKcContext({
  // Uncomment to test the login page for development.
  //mockPageId: "login.ftl",
});

export type KcContext = NonNullable&#x3C;ReturnType&#x3C;typeof getKcContext>["kcContext"]>;
</code></pre>
