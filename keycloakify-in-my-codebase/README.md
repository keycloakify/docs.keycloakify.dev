# 🔩 Integrating Keycloakify in your Codebase

{% hint style="info" %}
**Before You Start**:

This documentation section is relevant only if **you already have a project** and want to add a Keycloak theme as one of its deliverables.&#x20;

One of the powerful aspects of Keycloakify is its ability to let you reuse components and styles from your main application in your Keycloak theme. However, if you don't have an existing codebase, it’s easier to fork [the starter project](https://github.com/keycloakify/keycloakify-starter) and develop your Keycloak theme as a standalone project.
{% endhint %}

There is two main approach to integrate Keycloakify into your project, pick the one that you think will work best for you.

{% tabs %}
{% tab title="Collocation" %}
If you happen to be devlopping a React Single Page Application with Vite or Webpack you can install Keycloakify directly within your project!

This approach make it easy to reuse style and component of your main application in your Keycloakify theme if you are not in a mono-repository setup. &#x20;

{% content-ref url="in-your-react-project/" %}
[in-your-react-project](in-your-react-project/)
{% endcontent-ref %}
{% endtab %}

{% tab title="Monorepo" %}
There are many cases where the colocation apprach is not feasable, for example:

* You are using Next.js or another meta framwork that involves server side rendering.
* You are using a framework other than React (Vue, Angular, Svelt ...)

Beside, you might prefer to to keep your Keycloak theme as an isolated component of your app.

In this case you can integrate Keycloakify...

{% content-ref url="as-a-subproject-of-your-monorepo/" %}
[as-a-subproject-of-your-monorepo](as-a-subproject-of-your-monorepo/)
{% endcontent-ref %}
{% endtab %}
{% endtabs %}
