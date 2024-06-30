# As a Subproject of your Monorepo

{% tabs %}
{% tab title="Turborepo" %}
First you want to create a new subproject in your monorepo, just clone the starter template into apps/keycloak-theme.

```bash
cd my-turborepo
git clone https://github.com/keycloakify/keycloakify-starter apps/keycloak-theme
rm -rf apps/keycloak-theme/.git
rm -rf apps/keycloak-theme/.github
```

Change the name field in the package.json of your keycloakify sub app.

{% code title="apps/keycloak-theme/package.json" %}
```diff
 {
-    "name": "keycloakify-starter",
+    "name": "keycloak-theme",
 }
```
{% endcode %}

Give an actual name to your theme (as you want it to apprear [in the Keycloak Admin Console](https://github.com/keycloakify/keycloakify/assets/6702424/7da4afe2-0f67-4f79-a3d0-bd982636ea23))

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
<strong>    "build-keycloak-theme": "turbo run build-keycloak-theme --filter=keycloak-theme",
</strong><strong>    "start-keycloak": "keycloakify start-keycloak -p apps/keycloak-theme"
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
</strong><strong>        "outputs": [
</strong><strong>            "dist/**", 
</strong><strong>            "dist_keycloak/**"
</strong><strong>        ]
</strong><strong>    }
</strong>  }
}
</code></pre>

You can now build your keycloak theme at the root of your monorepo by running

```bash
npm run build-keycloak-theme
```

{% embed url="https://youtu.be/4h9lOf-4ZIE" %}
Building the theme, only compiling for Keycloak 25 with a custom jar file name. Demonstrating the effectiveness of turborepo cache
{% endembed %}

Optionally, if you want to change the location of the directory where the jar for your theme are created you can do: &#x20;

<pre class="language-typescript" data-title="apps/keycloak-theme/vite.config.ts"><code class="lang-typescript">export default defineConfig({
    plugins: [react(), keycloakify({
        themeName: "my-app",
<strong>        keycloakifyBuildDirPath: "../../dist/keycloak-theme"
</strong>    })]
});
</code></pre>

{% code title="turbo.json" %}
```diff
 {
   "$schema": "https://turbo.build/schema.json",
   "tasks": {
     // ... Other tasks
     "build-keycloak-theme": {
         "outputs": [
             "dist/**", 
-            "dist_keycloak/**"
+            "../../dist/keycloak-theme/**"
         ]
     }
   }
 }
```
{% endcode %}

If you applies thoses changes, when you'll run `npm run build-keycloak-theme` your JARs are going to be generated in `dist/keycloak-theme/`
{% endtab %}

{% tab title="Nx" %}
```bash
npx create-nx-workspace@latest --preset=react-monorepo --bundler=vite
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

```bash
cd nx-monorepo
rm -rf apps/keycloak-theme/src
git clone https://github.com/keycloakify/keycloakify-starter tmp
mv tmp/src apps/keycloak-theme
rm -rf tmp
```

<figure><img src="../.gitbook/assets/image (45).png" alt="" width="369"><figcaption></figcaption></figure>

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "@nx-monorepo/source",
  "version": "0.0.0",
  "dependencies": {
    "react": "18.3.1",
    "react-dom": "18.3.1",
    "tslib": "^2.3.0",
<strong>    "keycloakify": "10.0.0-rc.99"
</strong>  },
  "scripts": {
<strong>    "build-keycloak-theme": "nx build keycloak-theme &#x26;&#x26; keycloakify build -p apps/keycloak-theme",
</strong><strong>    "start-keycloak": "keycloakify start-keycloak -p apps/keycloak-theme"
</strong>  },
  // ...
</code></pre>

```bash
npm install # or `pnpm install` or `yarn`...
```

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">/// &#x3C;reference types='vitest' />
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { nxViteTsPaths } from '@nx/vite/plugins/nx-tsconfig-paths.plugin';
import { keycloakify } from "keycloakify/vite-plugin";


export default defineConfig({
  root: __dirname,
  cacheDir: '../../node_modules/.vite/apps/keycloak-theme',

  server: {
    port: 4200,
    host: 'localhost',
  },

  preview: {
    port: 4300,
    host: 'localhost',
  },

  plugins: [react(), nxViteTsPaths(), 
<strong>      keycloakify({
</strong><strong>          themeName: "my-project",
</strong><strong>          themeVersion: "1.0.0",
</strong><strong>          keycloakifyBuildDirPath: '../../dist/apps/keycloak-theme'
</strong><strong>       })
</strong>   ],

  // Uncomment this if you are using workers.
  // worker: {
  //  plugins: [ nxViteTsPaths() ],
  // },

  build: {
<strong>    outDir: 'dist',
</strong>    emptyOutDir: true,
    reportCompressedSize: true,
    commonjsOptions: {
      transformMixedEsModules: true,
    },
  },
});
</code></pre>

Now if you run `npm run build-keycloak-theme` it will generate the&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-06-30 at 10.19.42.png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="yarn/pnpm/npm/bun workspace" %}

{% endtab %}
{% endtabs %}

When you want to use the keycloakify CLI commands you can either cd into your keycloakify sub app directory or use the [--project option of the Keycloakify CLI](../build-options/project.md).  \
Like for example if you want to run add-story you can do either:

* `cd apps/keycloak-theme && npx keycloakify add-story`
* `npx keycloakify add-story -p apps/keycloakify-theme` from the root of your monorepo

To go beyond the base configuration you might want to expore what [build options](../build-options/) are available. Starting with with `keycloakVersionTargets` to make sure that you only generates the JARs file you need.

{% content-ref url="../targetting-specific-keycloak-versions.md" %}
[targetting-specific-keycloak-versions.md](../targetting-specific-keycloak-versions.md)
{% endcontent-ref %}

