---
icon: file-pen
---

# Terms and Conditions Page

The Tems and Condition feature of Keycloak enables you to make new users of your service consent to the terms of use of your services upon regisering. &#x20;

{% embed url="https://storybook.keycloakify.dev/?path=/story/login-terms-ftl--long-message" %}
Preview of the default terms.ftl page
{% endembed %}

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption><p>Preview of the the term and condition page.</p></figcaption></figure>

## Enabling the feature

If you want you show your terms and condition page when they create an account you have to enable it in your realm configuration. &#x20;

This is how to do it in the Keycloak Admin Console: &#x20;

* Navigat to your realm
* Left bar item: Authentication
* Tab: Required Actions
* Terms and condition: Enabled & Set as Default Action

## Defining your Terms and Conditions text

The way of defining your terms of services in Keycloak is to provide a message bundle for your realm that overrides the `termsText` key for the different languages that you have enabled. &#x20;

In recent Keycloak's versions this can be acheived directly via the Keycloak Admin Console as shown in this video: &#x20;

{% embed url="https://youtu.be/naW2TxwJZsA" %}

{% hint style="info" %}
If you want to use an iframe as text [read this](https://github.com/keycloakify/keycloakify/discussions/687).
{% endhint %}

## Customizing how the terms are rendered

If you want to customize the page that display the terms and condition you have to eject the [terms.ftl](https://storybook.keycloakify.dev/?path=/story/login-terms-ftl--default) page.

```bash
npx keycloakify eject-page
# Select login -> terms.ftl
```

This will create `src/login/Terms.tsx` in your project.

You also probably want to add a story for the Term page:

```bash
npx keycloakify add-story
# Select login -> terms.ftl
```

In `src/login/Terms.tsx`, note that `msg("termsText")` returns a `JSX.Element`. It's because the `msg()` function renders the string message as HTML text. You can't work directly with that. &#x20;

If you want to apply transformation to the text, you should use `msgStr("termsText")` instead. This returns the original string as defined in your realm configuration. &#x20;
