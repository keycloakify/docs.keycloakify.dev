---
icon: sailboat
---

# Using Tailwind

This demo show basic usage of tailwind in a Keycloakify login theme.

We show how you can levrage tailwind @apply directive to customize the default style without having to modify the components, just by tagetting the standardized kc classes.

We allso show how to use tailwind the regular way, at the component level on ejected pages.

Before you start with this exemple it is strongly recommended you read the CSS Level Customization guide. This will teach you how to partially or completely disable the PatternFly[^1] stlyes inherited from the default theme.

{% content-ref url="../css-customization.md" %}
[css-customization.md](../css-customization.md)
{% endcontent-ref %}



To use Tailwind in your Keycloakify project start by following the setup guide for Vite.

{% embed url="https://tailwindcss.com/docs/guides/vite#react" %}

Beyond that, here is a demo setup of light modification of the starter template to incorporate tailwind:

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/tailwind" %}


{% embed url="https://github.com/user-attachments/assets/efaa3fe7-aaf6-43cb-ae55-058858bd40ec" %}
Preview on the 'tailwind' branch for the starter template
{% endembed %}

What has been done:

* [Applying some custom tailwind utilities classes using the @apply directive](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L7-L14).
* Using the [Geist](https://vercel.com/font) font, [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/tailwind.config.js#L9-L11), [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L1) and [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L9).
* Ejecting the [login.ftl](https://storybook.keycloakify.dev/?path=/story/login-login-ftl--default) page (`npx keycloakify eject-page` and _login -> login.ftl_) and [applying a tailwind class](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/pages/Login.tsx#L172).

Here is the summary of the changes:

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/e6c71f13acbc65ccb8f57172c45e8c04a2151007" %}

[^1]: [PatternFly](https://v5-archive.patternfly.org/) is a utility based CSS framwork by RedHat in akin to [Bootstrap CSS](https://getbootstrap.com/docs/3.4/css/).  \
    It is used by the Keycloak team to build all it's UI.
