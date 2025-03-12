---
icon: puzzle
---

# Integrating Keycloakify in your Codebase

{% hint style="warning" %}
This is for advanced users. If you are just trying to get started with Keycloakify follow [the Quick Start Guide and fork the starter project.](../#quick-start)
{% endhint %}

There are two main approaches to integrate Keycloakify into your project.

## First Option: Installing Keycloakify Directly in Your SPA

{% hint style="danger" %}
🚨 **WARNING: ADVANCED USERS ONLY** 🚨

If you're unsure what this section is about, **this approach is NOT for you.** Instead, follow [the Quick Start Guide and fork the starter project.](../#quick-start)

This section is **only** for developers who already have an existing project and need to integrate a Keycloak theme **within it** to reuse existing components and styles.

🔹 If you're just trying to get started with Keycloakify, **stop here**—[the starter projects](../#quick-start) provide a much simpler and recommended path.

🔹 If you proceed without fully understanding how this approach differs from the starter project, you will likely get confused about what you’re actually doing, attempt to _simplify_ things, and end up hitting a roadblock.
{% endhint %}

If you are developing a [Single Page Application (SPA)](#user-content-fn-1)[^1], you can install Keycloakify directly within your project.

The main advantage of this approach is that your theme's source files will reside inside `src/keycloak-theme`, allowing you to directly import and reuse the components from your existing codebase.

This approach is suitable if your project falls into one of the following categories:

* Vite + React
* Create-React-App
* [Vite + Svelte](#user-content-fn-2)[^2]
* Webpack + React ⚠[^3]

Follow the guide corresponding to your setup:

{% content-ref url="vite.md" %}
[vite.md](vite.md)
{% endcontent-ref %}

{% content-ref url="webpack.md" %}
[webpack.md](webpack.md)
{% endcontent-ref %}

## Second Option: Setting Up Keycloakify as a Subproject in Your Monorepo

If your project is structured as a monorepo, you can add your Keycloak theme as a subproject, typically located at `apps/keycloak-theme`.

Choose the guide that matches your monorepo setup:

{% content-ref url="package-manager-workspaces.md" %}
[package-manager-workspaces.md](package-manager-workspaces.md)
{% endcontent-ref %}

{% content-ref url="turborepo.md" %}
[turborepo.md](turborepo.md)
{% endcontent-ref %}

{% content-ref url="nx.md" %}
[nx.md](nx.md)
{% endcontent-ref %}

{% content-ref url="angular-workspace.md" %}
[angular-workspace.md](angular-workspace.md)
{% endcontent-ref %}

[^1]: If your project is build with Next.js or Remix it does not fall under this cathegory.

[^2]: Svelte projects initialized with SvelteKit 1.0.0 or later use Vite as the default build tool. If your project was created in 2023 or later, it is likely based on Vite.

[^3]: Requires some advanced configuration
