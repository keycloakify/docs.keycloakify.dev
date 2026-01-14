---
icon: sailboat
---

# Shadcn UI (Tailwind)

{% columns %}
{% column %}
Theme type:

[Login](https://docs.keycloakify.dev/theme-types/difference-between-login-themes-and-the-other-types-of-themes)
{% endcolumn %}

{% column %}
Framework:

[React](https://react.dev/)
{% endcolumn %}

{% column %}
Base UI Toolkit:

[Tailwind](https://tailwindcss.com/) + [ShadCN UI](https://ui.shadcn.com/)
{% endcolumn %}
{% endcolumns %}

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://oussemasahbeni.github.io/keycloakify-shadcn-starter/" %}
Storybook Preview
{% endembed %}

## Setup Guide

{% stepper %}
{% step %}
### Create a new Vite Project

{% tabs %}
{% tab title="npm" %}
```bash
npm create vite@latest
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn create vite
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm create vite
```
{% endtab %}

{% tab title="bun" %}
```bash
bun create vite
```
{% endtab %}
{% endtabs %}

When prompted:

* **Project name:** `keycloak-theme` (or your preferred name)
* **Select a framework:** Choose **React**
* **Select a variant:** Choose **TypeScript**

```bash
cd keycloak-theme
```
{% endstep %}

{% step %}
### Add the Dependencies

{% tabs %}
{% tab title="npm" %}
```bash
npm install keycloakify @oussemasahbeni/keycloakify-login-shadcn
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add keycloakify @oussemasahbeni/keycloakify-login-shadcn
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add keycloakify @oussemasahbeni/keycloakify-login-shadcn
```
{% endtab %}

{% tab title="bun" %}
```bash
bun add keycloakify @oussemasahbeni/keycloakify-login-shadcn
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Initialize Keycloakify

{% tabs %}
{% tab title="npm" %}
```bash
npx keycloakify init
```
{% endtab %}

{% tab title="yarn" %}
```bash
npx keycloakify init
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm exec keycloakify init
```
{% endtab %}

{% tab title="bun" %}
```bash
bunx keycloakify init
```
{% endtab %}
{% endtabs %}

When prompted:

* **Which theme type would you like to initialize?** Select **(x) login**
* **Do you want to install the Stories?** Select **(x) Yes (Recommended)**
{% endstep %}

{% step %}
### Update the Vite Configuration

Changes that need to be made to your Vite config to enable Tailwind and Shadcn to work.

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";
import { defineConfig } from "vite";
import path from "node:path";
<strong>import tailwindcss from "@tailwindcss/vite";
</strong>
// https://vite.dev/config/
export default defineConfig({
    plugins: [
        react(),
<strong>        tailwindcss(),
</strong>        keycloakify({
            accountThemeImplementation: "none"
        })
    ],
<strong>    resolve: {
</strong><strong>        alias: {
</strong><strong>            "@": path.resolve(__dirname, "src")
</strong><strong>        }
</strong><strong>    }
</strong>});
</code></pre>
{% endstep %}

{% step %}
### Update TypeScript Paths

This is to make sure that you can import relative to the source with `"@/components/..."`

<pre class="language-json" data-title="tsconfig.json"><code class="lang-json">{
    // ...
    "compilerOptions": {
        // ...
<strong>        "paths": {
</strong><strong>            "@/*": ["./src/*"]
</strong><strong>        }
</strong>    }
}
</code></pre>

<pre class="language-json" data-title="tsconfig.app.json"><code class="lang-json">{
    "compilerOptions": {
        // ...
<strong>        "paths": {
</strong><strong>            "@/*": ["./src/*"]
</strong><strong>        }
</strong>    },
    "include": ["src"]
}
</code></pre>
{% endstep %}

{% step %}
### Initializing Git

Using Git is a requirement. It enables Keycloakify to track the files you have customized vs the files that you're just inheriting from the `keycloakify-login-shadcn` NPM package.

```bash
git init .
git add -A
git commit -m "Initial commit"
```
{% endstep %}

{% step %}
### Know the key commands

{% tabs %}
{% tab title="npm" %}
```bash
# Run Storybook for component development and testing
npm run storybook

# Build the JAR file to import in Keycloak, see https://docs.keycloakify.dev/deploying-your-theme
npm run build-keycloak-theme

# Test in a local Keycloak. More info: https://docs.keycloakify.dev/testing-your-theme/inside-of-keycloak
npx keycloakify start-keycloak
```
{% endtab %}

{% tab title="yarn" %}
```bash
# Run Storybook for component development and testing
yarn storybook

# Build the JAR file to import in Keycloak, see https://docs.keycloakify.dev/deploying-your-theme
yarn build-keycloak-theme

# Test in a local Keycloak. More info: https://docs.keycloakify.dev/testing-your-theme/inside-of-keycloak
npx keycloakify start-keycloak
```
{% endtab %}

{% tab title="pnpm" %}
```bash
# Run Storybook for component development and testing
pnpm storybook

# Build the JAR file to import in Keycloak, see https://docs.keycloakify.dev/deploying-your-theme
pnpm build-keycloak-theme

# Test in a local Keycloak. More info: https://docs.keycloakify.dev/testing-your-theme/inside-of-keycloak
pnpm exec keycloakify start-keycloak
```
{% endtab %}

{% tab title="bun" %}
```bash
# Run Storybook for component development and testing
bun run storybook

# Build the JAR file to import in Keycloak, see https://docs.keycloakify.dev/deploying-your-theme
bun run build-keycloak-theme

# Test in a local Keycloak. More info: https://docs.keycloakify.dev/testing-your-theme/inside-of-keycloak
bunx keycloakify start-keycloak
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Understanding how to Apply Your Changes

We highly recommend, before you start, to give this video a quick watch. It will tell you the key info you need to know to understand the Keycloakify Framework.

{% hint style="info" %}
NOTE: In this video, the demo is made with [`@keycloakify/login-ui`](https://www.npmjs.com/package/@keycloakify/login-ui), the default base UI that mirrors the Keycloak built-in theme.\
But here you are using [`@oussemasahbeni/keycloakify-login-shadcn`](https://www.npmjs.com/package/@oussemasahbeni/keycloakify-login-shadcn).

As a consequence, you can skip the part of the talk that addresses CSS level customization. It doesn't really apply here.\
The important part to understand is how to own the file that you want to modify.
{% endhint %}

{% embed url="https://youtu.be/0peJITq1WXU?si=SC-fhbepab5U00q8&t=245" %}
{% endstep %}
{% endstepper %}

## Source Code

Source code of [`@oussemasahbeni/keycloakify-login-shadcn`](https://www.npmjs.com/package/@oussemasahbeni/keycloakify-login-shadcn), the NPM package that powers this theme. Give it a star ⭐️!

{% embed url="https://github.com/Oussemasahbeni/keycloakify-shadcn-starter" %}
