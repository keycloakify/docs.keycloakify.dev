# Multi-Page

## Initializing the Multi-Page Account Theme

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>The Multi-Page Account theme before customization</p></figcaption></figure>

You've made your mind and opted for the Multi-Page Account theme?  \
Great, let's start by initializing you theme: &#x20;

```bash
npx keycloakify initialize-account-theme
```

When asked, select "Multi-Page". &#x20;

This command will create the nessesary boilerplate for you. &#x20;

Beyond that there isn't much thing you need to be aware of, things works exactly as in the login theme. You'll be able to use the  keycloakify`add-story` and `eject-page` CLI command just select account when asked.

## Using the REST API

{% hint style="warning" %}
At this stage this is meerly a POC. We'll provide better tooling in the near future.&#x20;
{% endhint %}

Even if you're using the Multi-Page theme you can still consume the REST API the Single-Page Account is build on top of. So, if some information you need are missing from the `kcContext` you can fetch them dynamically.

While this support is still a bit rough around the edges, we are working to better integrate it. Here is a minimal demo of how to call the API:

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/account_api_poc" %}
Branch of the starter template modified to call the Account REST API
{% endembed %}

Here are the relevant changes:

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/e4d4562b0e78b9b27afdb7b756229af21d7543c7" %}
The commit where the API call was added
{% endembed %}

You can find the code for the Account v3 theme [here](https://github.com/keycloak/keycloak/tree/main/js/apps/account-ui/src/api). This will help you infer all the available endpoints. You can also enable the Account v3 theme in your Keycloak and use the network tab to see the available endpoints.

If you run `npx keycloakify start-keycloak` in the modified starter, login with the test user, click on the account link, and open the dev tools, you'll see a logged response to an Account REST API call:

<figure><img src="../.gitbook/assets/image (51).png" alt="API Call Response"><figcaption><p>Console logging the response of GET <code>/account/?userProfileMetadata=true</code></p></figcaption></figure>
