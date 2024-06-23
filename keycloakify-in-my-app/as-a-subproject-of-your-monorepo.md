# As a Subproject of your Monorepo

{% tabs %}
{% tab title="Turborepo" %}
First you want to create a new subproject in your monorepo, just clone the starter template into apps/keycloak-theme.

```bash
cd my-turborepo
git clone https://github.com/keycloakify/keycloakify-starter apps/keycloak-theme
rm -r apps/keycloak-theme/.git
rm -r apps/keycloak-theme/.github
```

Change the name field in your package.json

{% code title="apps/keycloak-theme/package.json" %}
```diff
 {
-    "name": "keycloakify-starter",
+    "name": "keycloak-theme",
 }
```
{% endcode %}

Give an actual name to your theme (as it will apprear in the Keycloak Admin Console)

<pre class="language-typescript" data-title="apps/keycloak-theme/vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react(), keycloakify({
<strong>        name: "my-project"
</strong>    })]
});
</code></pre>

Then you want to add a new script for building your theme in your root **package.json**

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "my-turborepo",
  "scripts": {
    "build": "turbo build",
    "dev": "turbo dev",
    "lint": "turbo lint",
    "format": "prettier --write \"**/*.{ts,tsx,md}\"",
<strong>    "build-keycloak-theme": "turbo run build-keycloak-theme --filter=keycloak-theme"
</strong>  },
  // ...
}
</code></pre>

Add a turborepo task

<pre class="language-json" data-title="turbo.json"><code class="lang-json">{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    // ... Other tasks
<strong>    "build-keycloak-theme": {
</strong><strong>        "outputs": ["dist/**", "dist_keycloak/**"]
</strong><strong>    }
</strong>  }
}
</code></pre>

You can now build your keycloak theme at the root of your monorepo by running

```bash
npm run build-keycloak-theme
```

{% embed url="https://youtu.be/4h9lOf-4ZIE" %}
Building the theme, only compiling for Keycloak 25 with a custom jar file name
{% endembed %}
{% endtab %}

{% tab title="Nx" %}

{% endtab %}

{% tab title="yarn/pnpm/npm/bun workspace" %}

{% endtab %}
{% endtabs %}



When you want to run the npx keycloakify start-keycloak or npx keycloakify eject-page be sure to navigate first in your keycloakify sub project or use the --project CLI option. &#x20;

```bash
cd apps/keycloak-theme
npx keycloakify start-keycloak
# OR
npx keycloakify start-keycloak -p apps/keycloak-theme
```
