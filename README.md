# 👨‍💻 Quick start

{% hint style="warning" %}
Some section of this documentation website are still under construction. If you have any question don't hesitate to [reach out on Discord](https://discord.gg/kYFZG7fQmn).
{% endhint %}

Keycloakify is a tool that enables to create keycloak themes for customizing of the look and feel of Keycloak's user-facing pages. You can preview these pages here:&#x20;

{% embed url="https://storybook.keycloakify.dev/" %}

Why would I chose to use this thrid party tool insted of directly implementing [the theming system featured by Keycloak](https://www.keycloak.org/docs/latest/server\_development/#\_themes)?&#x20;

* Keycloakify lets you use modern frontend technology: **TypeScript**, **React**, and **any styling solution or component library** you'd like, such as **Tailwind**, **MUI**, **ShadeCN/UI**, or just **plain CSS**. With the base theming system you have to write [FreeMarker](https://freemarker.apache.org/index.html) and integrating frontend technologies into a Java Stack isn't straight forward.
* Keycloakify makes it very easy to test your theme [inside](testing-your-theme/in-a-keycloak-docker-container.md) and [outside](testing-your-theme/in-storybook.md) Keycloak with hot reloading enabled.
* Keycloakify bundles the theme for you into a JAR that you can simply [import into Keycloak](importing-your-theme-in-keycloak.md)t.
* The Keycloak themes generated with Keycloakify are compatible with all Keycloak versions down to Keycloak version 11, by oposition to regular themes that must be updated to target a specific Keycloak version.
* Keycloakify themes implement real-time frontend validation out of the box. For example, when a user chooses a password that is too weak, they see the feedback  message like _"the password must be at least 12 character long"_ immediately and not after they have pressed the submit button.
* We're here to help! Either via [our Discord](https://discord.gg/kYFZG7fQmn) channel or [GitHub issues](https://github.com/keycloakify/keycloakify/issues/new).

First thing you want to do is to fork/clone the Keycloakify Vite[^1] starter project.&#x20;

{% embed url="https://github.com/keycloakify/keycloakify-starter" %}

Then you can move on to the next section of the documentation:

{% content-ref url="testing-your-theme/" %}
[testing-your-theme](testing-your-theme/)
{% endcontent-ref %}

[^1]: There's also a Webpack based starter that you can find [here](https://github.com/keycloakify/keycloakify-starter-webpack).
