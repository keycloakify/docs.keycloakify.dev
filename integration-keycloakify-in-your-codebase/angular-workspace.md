# Angular Workspace

{% hint style="danger" %}
If you're unsure what this section is about, **this approach is NOT for you.** Instead, follow [the Quick Start Guide and fork the starter project.](../#quick-start)
{% endhint %}

## Integrating Keycloakify into an Angular Workspace

Let's assume you have a monorepo project where sub applications are stored in the **projects/** directory.

Next up you want to repatriate the Keycloakify Starter template sources.

```bash
cd my-workspace
yarn ng generate application keycloak-theme
rm -rf projects/keycloak-theme/public
rm -rf projects/keycloak-theme/src
git clone https://github.com/keycloakify/keycloakify-starter-angular-vite tmp
mv tmp/src projects/keycloak-theme
mv tmp/public projects/keycloak-theme
mv tmp/vite.config.ts projects/keycloak-theme
mv tmp/index.html projects/keycloak-theme
rm -rf tmp
```

#### Adjust the `vite.config.ts` File

Edit the highlighted path in `vite.config.ts` file to match the following configuration:

<pre class="language-javascript" data-title="vite.config.ts"><code class="lang-javascript">/// 

import { defineConfig } from 'vite';
import angular from '@analogjs/vite-plugin-angular';
import { keycloakify } from 'keycloakify/vite-plugin';

// https://vitejs.dev/config/
export default defineConfig(({ mode }) => ({
  build: {
    target: ['es2022'],
  },
<strong>  root: 'projects/keycloak-theme',
</strong>  resolve: {
    mainFields: ['module'],
  },
  plugins: [
    angular(),
    keycloakify({
      accountThemeImplementation: 'none',
<strong>      themeName: 'keycloak-theme',
</strong><strong>      keycloakifyBuildDirPath: '../../dist/keycloak-theme'
</strong>    }),
  ],
  define: {
    'import.meta.vitest': mode !== 'production',
  },
}));

</code></pre>

after this, your project structure should look like the following:

```
my-workspace/

│
├── projects/
│   └── keycloak-theme/
│       ├── src/
│       ├── public/
│       ├── index.html
│       ├── tsconfig.app.json
│       ├── tsconfig.spec.json
│       └── vite.config.ts
│
├── angular.json
└── package.json

```

### Update `package.json` with Keycloakify Configuration

Ensure the following lines are present in your workspace's `package.json`:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "name": "my-workspace",
  "version": "0.0.0",
<strong>  "type": "module",
</strong>  "scripts": {
    ...
<strong>    "build-keycloak-theme": "ng build --project keycloak-theme &#x26;&#x26; npx keycloakify build --project projects/keycloak-theme",
</strong>    ...
  },
  "dependencies": {
    ...
<strong>    "@keycloakify/angular": "^0.2.14",
</strong><strong>    "keycloakify": "^11.8.1",
</strong><strong>    "marked": "^5.0.2",
</strong><strong>    "marked-gfm-heading-id": "^3.0.4",
</strong><strong>    "marked-highlight": "^2.0.1",
</strong><strong>    "marked-mangle": "^1.1.7",
</strong><strong>    "prismjs": "^1.29.0",
</strong>    ...
  },
  "devDependencies": {
    ...
<strong>    "@analogjs/platform": "^1.11.0",
</strong><strong>    "@analogjs/vite-plugin-angular": "^1.9.0",
</strong><strong>    "@analogjs/vitest-angular": "^1.9.0",
</strong><strong>    "@angular-devkit/architect": "^0.1900.6",
</strong><strong>    "@nx/angular": "^20.3.0",
</strong><strong>    "@nx/devkit": "^20.3.0",
</strong><strong>    "@nx/vite": "^20.3.0",
</strong><strong>    "vite": "^5.0.0",
</strong><strong>    "vite-tsconfig-paths": "^4.2.0",
</strong><strong>    "vitest": "^2.0.0"
</strong>    ...
  }
}
</code></pre>

### Add the Keycloakify Project to `angular.json`

To integrate the **Keycloakify** project into your workspace, update the `angular.json` file by adding an entry to the `projects` section. Below is an example configuration. Important lines that may require customization based on your project’s requirements are highlighted:

<pre class="language-json" data-title="angular.json"><code class="lang-json">  ...
    "keycloak-theme": {
      "projectType": "application",
      "schematics": {},
<strong>      "root": "projects/keycloak-theme",
</strong><strong>      "sourceRoot": "projects/keycloak-theme/src",
</strong><strong>      "prefix": "kc",
</strong>      "architect": {
        "build": {
<strong>          "builder": "@analogjs/platform:vite",
</strong>          "options": {
<strong>            "configFile": "projects/keycloak-theme/vite.config.ts",
</strong><strong>            "outputPath": "projects/keycloak-theme/dist",
</strong><strong>            "main": "projects/keycloak-theme/src/main.ts",
</strong><strong>            "index": "projects/keycloak-theme/index.html",
</strong><strong>            "tsConfig": "projects/keycloak-theme/tsconfig.app.json"
</strong>          },
          "configurations": {
            "production": {
              "budgets": [
                {
                  "type": "initial",
                  "maximumWarning": "500kB",
                  "maximumError": "1MB"
                },
                {
                  "type": "anyComponentStyle",
                  "maximumWarning": "4kB",
                  "maximumError": "8kB"
                }
              ],
              "outputHashing": "all"
            },
            "development": {
              "optimization": false,
              "extractLicenses": false,
              "sourceMap": true
            }
          },
          "defaultConfiguration": "production"
        },
        "serve": {
<strong>          "builder": "@analogjs/platform:vite-dev-server",
</strong>          "configurations": {
            "production": {
              "buildTarget": "keycloak-theme:build:production",
<strong>              "port": 5173
</strong>            },
            "development": {
              "buildTarget": "keycloak-theme:build:development",
              "hmr": true
            }
          },
          "defaultConfiguration": "development"
        },
        "lint": {
          "builder": "@angular-eslint/builder:lint",
          "options": {
            "lintFilePatterns": ["src/**/*.ts", "src/**/*.html"]
          }
        }
      }
    }
</code></pre>

## Use Keycloakify

The application should now be good to go. Make sure that whenever you run a `npx keycloakify` command in your workspace root you add the path to your keycloakify project like this:\
`npx keycloakify build --project projects/keycloak-theme`
