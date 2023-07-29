# 🔧 Environnement Variables

Environment variables defined on the Keycloak server can be transferred to the theme. This allows for a degree of theme customization without necessitating a rebuild. This approach is particularly useful if multiple parties are reusing your theme. As an example, you can distribute a single .jar file to multiple customers, enabling them to modify certain aspect of the login page by defining specific environment variables.

While this feature is not yet fully documented, you can glean some understanding by referring to the build options provided below. This should assist you in making sense of the functionality:

{% embed url="https://docs.keycloakify.dev/build-options#extrathemeproperties" %}
The extraThemeProperty build option
{% endembed %}
