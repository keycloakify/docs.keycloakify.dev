# Nx Integrated Monorepo

Let's see how to integrate a Keycloakify theme into a Nx project with integrated monorepo.

In this example we'll start with the Nx Vite starter

```bash
npx create-nx-workspace@latest --preset=react-monorepo --bundler=vite
```

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

Next up we want to repatriate the Keycloakify Starter template sources.\
We only copy over the src and .storybook directory.

```bash
cd nx-monorepo
rm -rf apps/keycloak-theme/src
git clone https://github.com/keycloakify/keycloakify-starter tmp
mv tmp/src apps/keycloak-theme
mv tmp/.storybook apps/keycloak-theme
rm -rf tmp
```

<figure><img src="../../.gitbook/assets/image (46).png" alt="" width="365"><figcaption><p>After moving src and .storybook to apps/keycloak-theme</p></figcaption></figure>

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
<strong>    "keycloakify": "10.0.0-rc.106"
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

<pre class="language-typescript" data-title="apps/keycloak-theme/vite.config.ts"><code class="lang-typescript">/// &#x3C;reference types='vitest' />
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

<figure><img src="../../.gitbook/assets/Screenshot 2024-06-30 at 12.29.58.png" alt=""><figcaption></figcaption></figure>

When you want to use the keycloakify CLI commands you can either cd into your keycloakify sub app directory or use the [--project option of the Keycloakify CLI](../../configuration-options/project.md).\
Like for example if you want to run add-story you can do either:

* `cd apps/keycloak-theme && npx keycloakify add-story`
* `npx keycloakify add-story -p apps/keycloakify-theme` from the root of your monorepo

To go beyond the base configuration you might want to explore what [build options](../../configuration-options/) are available. Starting with with `keycloakVersionTargets` to make sure that you only generates the JARs file you need.

{% content-ref url="../../targeting-specific-keycloak-versions.md" %}
[targeting-specific-keycloak-versions.md](../../targeting-specific-keycloak-versions.md)
{% endcontent-ref %}
