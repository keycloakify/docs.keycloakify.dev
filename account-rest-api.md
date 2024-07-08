# 🔌 Account REST API

{% hint style="warning" %}
Account v3 is soon to be integrated to Keycloakify.  \
![](<.gitbook/assets/image (63).png>)

Currently [Keycloakify is using Account v1](https://storybook.keycloakify.dev/?path=/story/account-account-ftl--default).  \
If you're interested by this you might want to wait until account v3 is integrated. &#x20;
{% endhint %}

The Keycloak Account v3 (the default account theme that comes with Keycloak) is built on top of a REST API. You can consume this API from your Keycloakify Account theme! So, if you feel limited by the `kcContext` you get in the Account pages, you can leverage this API.

While this support is still a bit rough around the edges, we are working to better integrate it. Here is a minimal demo of how to call the API:

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/account_api_poc" %}
Branch of the starter template modified to call the Account REST API
{% endembed %}

Here are the relevant changes:

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/134c60610f1e367b6495ecb7bd0743bb2d73a472" %}
The commit where the API call was added
{% endembed %}

You can find the code for the Account v3 theme [here](https://github.com/keycloak/keycloak/tree/main/js/apps/account-ui/src/api). This will help you infer all the available endpoints. You can also enable the Account v3 theme in your Keycloak and use the network tab to see the available endpoints.

If you run `npx keycloakify start-keycloak` in the modified starter, login with the test user, click on the account link, and open the dev tools, you'll see a logged response to an Account REST API call:

<figure><img src=".gitbook/assets/image (51).png" alt="API Call Response"><figcaption><p>Console logging the response of GET <code>/account/?userProfileMetadata=true</code></p></figcaption></figure>
