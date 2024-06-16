# 🙋 How do I add extra pages?

You can't add pages per say. The theme isn't in contôle of the authentication cinematic. The Keycloak server defines the login/registration steps all you can do is customise the pages defined by Keycloak. &#x20;

That beeing said. If what you are trying to do is implementing, for example a multi step registration process you can do this! You'll do it at the page level by dynamically swapping react components.

If you have implemented a custom Keycloak extension (in Java) that does define some non standard extra user facing pages you can implement implement them in your Keycloakify theme. See:

{% content-ref url="../integration-with-custom-keycloak-extention.md" %}
[integration-with-custom-keycloak-extention.md](../integration-with-custom-keycloak-extention.md)
{% endcontent-ref %}
