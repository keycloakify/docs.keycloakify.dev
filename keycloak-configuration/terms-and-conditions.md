# 📄 Terms and conditions

The Tems and Condition feature of Keycloak enalbes you to make new users of your service consent to the terms of use of your services upon regisering. &#x20;

{% embed url="https://storybook.keycloakify.dev/?path=/story/login-terms-ftl--default" %}

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Enabling the feature

If you want you show your terms and condition page when they create an account you have to enable it in your realm configuration. &#x20;

This is how to do it in the Keycloak Admin Console: &#x20;

TODO: Tango

## Defining your Terms and Conditions text

The way of defining your terms of services in Keycloak is to provide a message bundle for your realm that overrides the `temsText` key for the different languages that you have enabled. &#x20;

In recent Keycloak's versions this can be acheived directly via the Keycloak Admin Console as shown in this video: &#x20;

{% embed url="https://youtu.be/naW2TxwJZsA" %}

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
