# 🔌 Account REST API

As explained in [this post](https://github.com/keycloakify/keycloakify/discussions/346#discussioncomment-5889791), Keycloakify Account theme implements the same approach than the login theme.  \
This is to make the knowelage you have garner from customizing the account theme transefable to the account theme customization.  \
However the account theme that comes out of the box with Keycloakify is somewhat outdated (we're working on this) and is not using all the latest features that Keycloak offers.  \
The Keycloak Account v3 (the default account theme that comes with Keycloak) is build on top of a REST API but don't worry, you can consume this API from your Keycloakify Account theme!  So if you feel limitated by the kcContext you get in the Account pages you can levrage this API.  \
\
This support is still a bit rough on the edges and we're working to better integrate it but anyway here is a minimal demo of how to call the the API: &#x20;

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/account_api_poc" %}
Branch of the starter template modified to call the Account REST API
{% endembed %}

This is the relevent changes:

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/aaab4b803c5debf3e9400ac1d40c0dcec8653c19" %}
The commit where the API call was added
{% endembed %}

[Here](https://github.com/keycloak/keycloak/tree/main/js/apps/account-ui/src/api) you have the code from the Account v3 theme from which you can infer all the endpoints available, you can also enable the Account v3 theme in your Keycloak and use the network tab to see what are the endpoints available.

One important thing to note is that you need to add /resources/\* to the valid redirect URIs of the account-console client in your Realm configuration (it wont be nessesary in the future): &#x20;

{% embed url="https://app.tango.us/app/embed/b5185d2d-12ff-41a6-8851-7f6ea2abfa75" %}

If you run `npx keycloakify start-keycloak` in the modified starter, login with the test user, click on the account link and open the dev tools, you'll see logged a response to an Account REST API call:

<figure><img src=".gitbook/assets/image (51).png" alt=""><figcaption><p>Console.loging the response of GET <code>/account/?userProfileMetadata=true</code></p></figcaption></figure>
