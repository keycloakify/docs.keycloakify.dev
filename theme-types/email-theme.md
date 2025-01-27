---
icon: envelope
---

# Email Theme

There are two ways you can create a Keycloak Email theme with Keycloakify

## Using keycloakify-emails

[keycloakify-email](https://github.com/timofei-iatsenko/keycloakify-emails) is a Keycloakify plugin that enable to create an email theme using [jsx-email](https://jsx.email/) or any other email templating solution. &#x20;

{% embed url="https://github.com/garronej/keycloakify-emails/tree/email_dir_location" %}

_This plugin will evenutally be integrated to Keycloakify core._  \
\

{% hint style="warning" %}
This approach only works in Vite project. So not with Webpack/Create-React-App
{% endhint %}

{% tabs %} {% tab title="React" %}

yarn add keycloakify-emails jsx-email
{% endtab %}

{% tab title="Svelte" %}

yarn add keycloakify-emails @keycloakify/svelte-email
{% endtab %} {% endtabs %}

For more instruction on how to configure Keycloak to send emails [see this video](https://www.youtube.com/watch?v=IZ9LSLfWxqo\&t=177s).

## Unsing FreeMarker

`npx keycloakify initialize-email-theme`, select the `native` option.

Running this command will initialize a native email theme in the `src/email` directory.

{% embed url="https://youtu.be/IZ9LSLfWxqo" %}
