# 🔩 Integrating Keycloakify in your Project

This documentation section is rellevent to you if you have already a web project and you want to integrate a Keycloak theme as an integral part of your project. &#x20;

One of the powerfull aspect of Keycloakify is to enable you to reuse component style and logic of your main application in your Keycloak theme. This enable for a seemless transition from your app to your Keycloak pages. &#x20;

For integrating Keycloakify into your project there's two strategies. &#x20;

## Collocation

If you happen to be devlopping a React Single Page Application with Vite or Webpack you can install Keycloakify directly within your project and have your theme source in a keycloak-theme sub directory of your src directory! &#x20;

{% content-ref url="in-your-vite-react-project.md" %}
[in-your-vite-react-project.md](in-your-vite-react-project.md)
{% endcontent-ref %}

## Monorepo

If the collocation approach is not an option for you either because your not building an SPA (you are using Next or Remix) or because you're using another frontend framwork than React you can integrate your Keycloakify as a subproject of your Monorepo.  \


## Redirecting your users to the Login/Register pages

If you are integrating Keycloakify into an existing app, chances are you've already figured out how to setup an OIDC client in your web app.  \
You are probably using [keycloak-js](https://www.npmjs.com/package/keycloak-js) (soon to be deprecated), [oidc-client-ts](https://github.com/authts/oidc-client-ts) or [react-oidc-context](https://github.com/authts/react-oidc-context). &#x20;

However, you might want to checkout oidc-spa, an alternative to the above mentioned library by the Keycloakify team!

{% embed url="https://www.oidc-spa.dev/" %}
