# 👨‍💻 Quick start

Keycloakify is a tool that enables to customize the look and feel of Keycloak's user facing pages. You can preview thoses pages here:   &#x20;

{% embed url="https://storybook.keycloakify.dev/" %}

Why do I need Keycloakify in the first place? Why can't I just implement the default theming system provided by Keycloak? &#x20;

* Keycloakify let's you use modern frontend technology: TypeScript, React and any styliing solution you'd like such as TailwindCSS, MUI, ShadeCN/UI or just plain CSS modules. Wheas the Keycloak theming system is based on the FreeMarker templating language.&#x20;
* Keycloakify make it very easy to test your theme inside and outside Keycloak with hot reloading enabled.
* Keycloakify bundles the theme for you into a JAR that you can simply import into Keycloak.
* The Keycloak themes generated with Keycloakify are compatible all keycloak Keycloak version down to v11 when regular theme must be updated to target a specific Keycloak version. &#x20;
* Keycloakify themes implements realtime frontend validation out of the box. When user choose a password that is too weak for example they see the feedback immediatly wheras with regular theme they only get the error after pressing submit. &#x20;
* Keycloakify is well documented, well maintained and you're wellcome to join our discord chanell if you're having difficulties. &#x20;

Convinced? Start playing with the starter project now and comme back here in a few minutes to learn more!

{% hint style="info" %}
Although successfull implementation of Keycloakify had already been acheived in Vite and Angular. Keycloakify is, as of today, primarely a React framwork.
{% endhint %}

{% embed url="https://github.com/codegouvfr/keycloakify-starter" %}

