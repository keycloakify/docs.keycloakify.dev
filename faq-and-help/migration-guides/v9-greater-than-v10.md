# ⬆️ v9 -> v10

A lot of things have changed in Keycloakify 10 compared to the previous versions. The software as almost been rewrote from scratch, it is now a much more mature solution but with it comes quites a few of breaking changes.    \
Keycloakify will be more stable going forward you shouldn't have to go through a painfull migration process to support the latest Keycloak version moving forward.&#x20;

A big breaking change whas nessesary this time to lays strong fundation for the future of the project.  \
\
I've recorded a video where I try to some up everything that have changed, I apologize, it's quite a long video, if you don't have the time to watch all of it you can probably go quicker by reading the new documentation:&#x20;

\<youtube embedded>

Here are the Key points adressed in this video:

## Update strategies

For updating my advise is to start fresh from the new starter and re apply your changes. &#x20;

* If you had only a Keycloak theme
  * Here is the Vite starter: [https://github.com/keycloakify/keycloakify-starter](https://github.com/keycloakify/keycloakify-starter)
  * Here is the Wepback (Create React App) starter: [https://github.com/keycloakify/keycloakify-starter-webpack](https://github.com/keycloakify/keycloakify-starter-webpack)
* If you had Keycloakify installed in your web app you can follow this integration guide: [Itegrating Keycloakify in your React project](../../keycloakify-in-my-codebase/in-your-react-project/).

## Key things that have changed:

### User Profile is now the default

* register-user-profile.ftl has been renamed register.ftl. The old register.ftl where the user attribute where hardcoded has been removed.
* update-user-profile.ftl has been renamed login-update-profile.ftl, the old login-update-profile.ftl has been removed.

Don't worry Keycloakify still generates theme that are compatible with older Keycloak versions, including version that didn't had the declarative User Profile features. &#x20;

### There are now two type of Account themes

You can now choose between the Single Page Account Theme or the Multi Page Account theme. [See documentation](../../account-theme/). &#x20;

If you had no account theme, or you didn't customized one, you must set accountThemeImplementation to "none" in your Keycloakify configs (and you can remove the src/keycloak-theme/account directory):

{% tabs %}
{% tab title="Vite" %}
<pre class="language-tsx" data-title="vite.config.ts"><code class="lang-tsx">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
<strong>            accountThemeImplementation: "none",
</strong><strong>            //...
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
        "accountThemeImplementation": "none"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

If you had an account theme, you must set accoutThemeImplementation to "Multi-Page":

{% tabs %}
{% tab title="Vite" %}
<pre class="language-tsx" data-title="vite.config.ts"><code class="lang-tsx">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
<strong>            accountThemeImplementation: "Multi-Page",
</strong><strong>            //...
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
        "accountThemeImplementation": "Multi-Page"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Keycloakify components API changes

There are a lot of little adjustments in the Keycloakify Component API and the boilerplate.  \
One of the more notable one is [useGetClassName](https://github.com/keycloakify/keycloakify-starter/blob/081c7d415088b022cb9595bd4bca3a502171ed3a/src/keycloak-theme/login/pages/Login.tsx#L18-L21) that is now [getKcClsx](https://github.com/keycloakify/keycloakify/blob/1785916d32f11b36527dbac6625643c5342149ab/src/login/pages/Login.tsx#L12-L15).  \
You can see the code for the new pages here: [https://github.com/keycloakify/keycloakify/tree/main/src/login/pages](https://github.com/keycloakify/keycloakify/tree/main/src/login/pages)  \
\
It's very important that you re-apply you changes to the new Template.tsx, and not try to make your old Template.tsx compile. See the new template here: [https://github.com/keycloakify/keycloakify/blob/main/src/login/Template.tsx](https://github.com/keycloakify/keycloakify/blob/main/src/login/Template.tsx) &#x20;
