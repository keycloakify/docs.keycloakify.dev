# ⬆️ v9 -> v10

**Keycloakify 10 has undergone significant changes compared to previous versions.** The software has been nearly rewritten from the ground up, making it a more mature and stable solution. However, this also brings several breaking changes. Moving forward, Keycloakify will be more stable, and you shouldn’t have to endure a painful migration process to support the latest Keycloak versions.

This major breaking change was necessary to lay a strong foundation for the future of the project.

I’ve recorded a video summarizing all the changes. It’s quite detailed, so if you don’t have time to watch the entire video, you can refer to the updated documentation for a quicker overview:

{% embed url="https://youtu.be/BQQO03lbS6s" %}

## Key Points Covered in the Video:

### Update Strategies

When updating, I recommend starting fresh with the new starter and reapplying your custom changes.

* **If you only had a Keycloak theme:**
  * Vite starter: [https://github.com/keycloakify/keycloakify-starter](https://github.com/keycloakify/keycloakify-starter)
  * Webpack (Create React App) starter: [https://github.com/keycloakify/keycloakify-starter-webpack](https://github.com/keycloakify/keycloakify-starter-webpack)
* **If you had Keycloakify installed in your web app:**\
  Follow this integration guide: [Integrating Keycloakify in your React project](../../keycloakify-in-my-codebase/in-your-react-project/).

### Key Changes:

#### User Profile is Now the Default

* The `register-user-profile.ftl` file has been renamed to `register.ftl`. The old `register.ftl`, where user attributes were hardcoded, has been removed.
* The `update-user-profile.ftl` file has been renamed to `login-update-profile.ftl`, and the old `login-update-profile.ftl` has also been removed.

Don’t worry—Keycloakify still generates themes that are compatible with older Keycloak versions, including those that did not have the declarative User Profile feature.

#### Two Types of Account Themes

You now have the option to choose between a Single Page Account Theme or a Multi-Page Account Theme. [See documentation](../../account-theme/).

If you haven't created or customized an account theme, set `accountThemeImplementation` to `"none"` in your Keycloakify configuration. You can also remove the `src/keycloak-theme/account` directory if it exists.

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

#### Keycloakify Components API Changes

The Keycloakify Component API and boilerplate have undergone several small adjustments.

One of the most notable changes is the renaming of `useGetClassName` to `getKcClsx`. You can view the updated implementation here: [useGetClassName](https://github.com/keycloakify/keycloakify-starter/blob/081c7d415088b022cb9595bd4bca3a502171ed3a/src/keycloak-theme/login/pages/Login.tsx#L18-L21) is now [getKcClsx](https://github.com/keycloakify/keycloakify/blob/1785916d32f11b36527dbac6625643c5342149ab/src/login/pages/Login.tsx#L12-L15).

For the latest code for the new pages, refer to this repository: [New Login Pages](https://github.com/keycloakify/keycloakify/tree/main/src/login/pages).

It's crucial that you reapply your changes to the new `Template.tsx` instead of trying to modify your old `Template.tsx` to compile. You can find the updated template here: [New Template.tsx](https://github.com/keycloakify/keycloakify/blob/main/src/login/Template.tsx).
