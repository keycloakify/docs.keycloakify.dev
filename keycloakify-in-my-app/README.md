# 🔩 Integrating Keycloakify in your Project

This documentation section is rellevent to you only if you have already a web project and you want to integrate a Keycloak theme into it. &#x20;

One of the powerfull aspect of Keycloakify is to enable you to reuse component style of your main application in your Keycloak theme. This enable for a seemless transition from your app to your Keycloak pages. &#x20;

For integrating Keycloakify into your project there's two strategies. &#x20;

## Collocation

If you happen to be devlopping a React Single Page Application with Vite or Webpack you can install Keycloakify directly within your project and have your theme source in a keycloak-theme sub directory of your src directory! &#x20;

{% content-ref url="in-your-vite-react-project.md" %}
[in-your-vite-react-project.md](in-your-vite-react-project.md)
{% endcontent-ref %}

## Monorepo

If the collocation approach is not an option for you either because your not building an SPA (you are using Next or Remix) or because you're using another frontend framwork than React you can integrate your Keycloakify as a subproject of your Monorepo.  \


{% content-ref url="as-a-subproject-of-your-monorepo/" %}
[as-a-subproject-of-your-monorepo](as-a-subproject-of-your-monorepo/)
{% endcontent-ref %}
