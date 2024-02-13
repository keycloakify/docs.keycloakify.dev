---
description: Setting up Keycloakify in your Web App
---

# 🔩 Keycloakify in my App

{% tabs %}
{% tab title="Vite" %}
If you are starting a new project, the best aproach is to fork the starter repo, read it's readme and start from here. &#x20;

{% embed url="https://github.com/keycloakify/keycloakify-starter" %}

If you are familiar with how Keycloakify work and you want to set it up in an existing Vite project here is what you need to do:

### Add it to your dependencies

```bash
yarn add keycloakify # Or npm install --save keycloakify
```

### Enable the Keycloakify's Vite plugin

<pre class="language-tsx" data-title="vite.config.ts"><code class="lang-tsx">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
<strong>import { keycloakify } from "keycloakify/vite-plugin";
</strong>
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(), 
<strong>    keycloakify()
</strong>  ]
})
</code></pre>

### Register the command to build your theme

<pre class="language-json" data-title="package.json"><code class="lang-json">...
  "scripts": {
    "dev": "vite",
    "build": "tsc &#x26;&#x26; vite build",
    "storybook": "storybook dev -p 6006",
<strong>    "build-keycloak-theme": "yarn build &#x26;&#x26; keycloakify"
</strong>  },
...
</code></pre>

{% hint style="info" %}
Monorepos: You can run Keycloakify from the root of your project with:&#x20;

`npx keycloakify --project <path>`

`<path>` would be tipically something like `packages/keycloak-theme`
{% endhint %}

### Storybook: List the public/ directory as static dir

<pre class="language-typescript" data-title=".storybook/main.ts"><code class="lang-typescript">import type { StorybookConfig } from "@storybook/react-vite";

const config: StorybookConfig = {
  // ...
<strong>  staticDirs: ["../public"]
</strong>};
export default config;
</code></pre>
{% endtab %}

{% tab title="Create React App (Webpack)" %}

{% endtab %}
{% endtabs %}



{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-keycloakify-in-my-app" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
