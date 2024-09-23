# 😞 Can't identify the page to customize?

If you can't seem to find the page that you're looking for in [the reference Storybook](https://storybook.keycloakify.dev/?path=/story/introduction--page), first thing to try is to open the devlopper tool and see the page id of the page you're trying to customize.  \


<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Identificating register.ftl</p></figcaption></figure>

Here there is two scenario:&#x20;

First one is there is no KcContext

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>No kcContext defined on the global</p></figcaption></figure>

If you get this error this means that the theme isn't correctly enabled on the client/realm. Checkout [this guide](../enabling-your-theme.md) or join [our discord server](https://discord.gg/kYFZG7fQmn), we'll help you debug.  \
\
Other scenario you get a page that is not listed in [the reference storybook](https://storybook.keycloakify.dev/?path=/story/introduction--page). This probably mean that you have a third party Keycloak extention enabled on your Keycloak server, you can still customize this page, just follow this guide: &#x20;

{% content-ref url="../styling-custom-extension-page.md" %}
[styling-custom-extension-page.md](../styling-custom-extension-page.md)
{% endcontent-ref %}
