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
<strong>        themeName: "my-app"
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
Let's see how to integrate a Keycloakify theme into a Nx project with integrated monorepo.

In this example we'll start with the Nx Vite starter

```bash
npx create-nx-workspace@latest --preset=react-monorepo --bundler=vite
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

Next up we want to repatriate the Keycloakify Starter template sources.  \
We only copy over the src and .storybook directory.

```bash
cd nx-monorepo
rm -rf apps/keycloak-theme/src
git clone https://github.com/keycloakify/keycloakify-starter tmp
mv tmp/src apps/keycloak-theme
mv tmp/.storybook apps/keycloak-theme
rm -rf tmp
```

<figure><img src="../.gitbook/assets/image (46).png" alt="" width="365"><figcaption><p>After moving src and .storybook to apps/keycloak-theme</p></figcaption></figure>

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "@nx-monorepo/source",
  "version": "0.0.0",
  "scripts": {
<strong>    "build-keycloak-theme": "nx build keycloak-theme &#x26;&#x26; keycloakify build -p apps/keycloak-theme",
</strong><strong>    "keycloak-theme-storybook": "npx storybook dev -p 6006 -c apps/keycloak-theme/.storybook",
</strong><strong>    "start-keycloak": "keycloakify start-keycloak -p apps/keycloak-theme"
</strong>  },
  "dependencies": {
    "react": "18.3.1",
    "react-dom": "18.3.1",
    "tslib": "^2.3.0",
<strong>    "keycloakify": "10.0.0-rc.99"
</strong>  },
  "devDependencies": {
<strong>      "storybook": "^8.1.10",
</strong><strong>      "@storybook/react": "^8.1.10",
</strong><strong>      "@storybook/react-vite": "^8.1.10"
</strong>  // ...
</code></pre>

```bash
npm install # or `pnpm install` or `yarn`...
```

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">/// &#x3C;reference types='vitest' />
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { nxViteTsPaths } from '@nx/vite/plugins/nx-tsconfig-paths.plugin';
<strong>import { keycloakify } from "keycloakify/vite-plugin";
</strong>
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

Now if you run `npm run build-keycloak-theme` it will generate the JAR in dist/apps/keycloak-theme.

<figure><img src="../.gitbook/assets/Screenshot 2024-06-30 at 12.29.58.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="yarn/pnpm/npm/bun workspace" %}
{% hint style="info" %}
In this section we assume we are using pnpm but this guide applies to any package manager that support namespaces. &#x20;
{% endhint %}



```bash
# pnpm
pnpm --filter keycloak-theme run build-keycloak-theme
# yarn
yarn workspace keycloak-theme run build-keycloak-theme
# npm
npm run build-keycloak-theme --workspace=keycloak-theme
# bun
bun run --cwd apps/keycloak-theme build-keycloak-theme
```

Let's assume we have a monorepo project where sub applications are stored in the apps/ directory.  \
For example with pnpm that would be the case if you have:

<pre class="language-yaml" data-title="pnpm-workspace.yaml"><code class="lang-yaml">packages:
<strong>  - 'apps/*'
</strong>  - 'packages/*'
</code></pre>



Then, you want to create a new app called, for example 'keycloak-theme' and initialize it with the code of the starter template: &#x20;

```bash
cd my-monorepo
git clone https://github.com/keycloakify/keycloakify-starter apps/keycloak-theme
rm -rf apps/keycloak-theme/.git
rm -rf apps/keycloak-theme/.github
rm apps/keycloak-theme/.yarn.lock
```

<figure><img src="../.gitbook/assets/image (50).png" alt="" width="375"><figcaption></figcaption></figure>

Now you want to update the name field of your apps/keycloak-theme/package.json to match the name of your sub app.&#x20;

{% code title="apps/keycloak-theme/package.json" %}
```diff
 {
-    "name": "keycloakify-starter",
+    "name": "keycloak-theme",
```
{% endcode %}

You also want to provide an actual name to your theme as you want it to [appear in the Keycloak Admin UI](https://github.com/keycloakify/keycloakify/assets/6702424/7da4afe2-0f67-4f79-a3d0-bd982636ea23).

<pre class="language-typescript" data-title="apps/keycloak-theme/vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react(), keycloakify({
<strong>        themeName: "my-app"
</strong>    })]
});
</code></pre>

Now you can add a script in your root package json to build the theme and start the keycloak dev server:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
   //...
   "scripts": {
<strong>       "build-keycloak-theme": "pnpm --filter keycloak-theme run build-keycloak-theme",
</strong><strong>       "start-keycloak": "keycloakify start-keycloak -p apps/keycloak-theme"
</strong>   }
}
</code></pre>

Now you can run:

```bash
pnpm install
pnpm run build-keycloak-theme
```

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

Two common thing you might want to do is [change the location of the directory where the JARs files are generated](../build-options/keycloakifybuilddirpath.md) and [only build the JAR for the Keycloak version you are using](../targetting-specific-keycloak-versions.md).

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react(), keycloakify({
        themeName: "my-app",
<strong>        keycloakifyBuildDirPath: "../../dist/apps/keycloak-theme",
</strong><strong>        keycloakVersionTargets: {
</strong><strong>            hasAccountTheme: true,
</strong><strong>            "21-and-below": false,
</strong><strong>            "23": false,
</strong><strong>            "24": false,
</strong><strong>            "25-and-above": "keycloak-theme.jar"
</strong><strong>        }
</strong>    })]
});
</code></pre>

In this configuration when you run `pnpm run build-keycloak-theme` from the root of your monorepo a single `keycloak-theme.jar` will be generated in **dist/apps/keycloak-theme**:

<figure><img src="../.gitbook/assets/Screenshot 2024-06-30 at 16.59.37.png" alt=""><figcaption></figcaption></figure>
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

