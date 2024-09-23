# Adding New Translation Messages Or Changing The Default Ones

Let's see firs how you can overwrite the default translation messages to best fit your usecases.

## At theme level

See, for example, by default the login page shows "Sign in to your account":

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="375"><figcaption></figcaption></figure>

Let's say we want to change that with a message more specific to you usecase.

First setp is to identify the message key. You can usually found it just by inspecting the HTML of your page:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Here we can see that the _"Sign in to your account"_ translation message corespond to the message key **loginAccountTitle**.

Let's change the English and French translations: &#x20;

{% code title="src/login/i18n.ts" %}
```typescript
import { i18nBuilder } from "keycloakify/login";
import type { ThemeName } from "../kc.gen";

/** @see: https://docs.keycloakify.dev/i18n */
const { useI18n, ofTypeI18n } = i18nBuilder
    .withThemeName<ThemeName>()
    .withExtraLanguages({ /* ... */ })
    .withCustomTranslations({
        en: {
            loginAccountTitle: "Log in to your ACME account"
        },
        // cspell: disable
        fr: {
            loginAccountTitle: "Connectez-vous a votre compte ACME"
        }
        // cspell: enable
    })
    .build();

type I18n = typeof ofTypeI18n;

export { useI18n, type I18n };
```
{% endcode %}

Here is the result that you should get:

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="warning" %}
The messageBundle that you provide as argument of the `createUseI18n` function must be statically valuable. You can't import from external files. All the translations must be declared inline.\
This is because Keycloakify will analyze your code at build time to make Keycloak aware of your modifications of the base messages so that server side generated feedback messages can use your translations.

![](<../.gitbook/assets/image (47).png>)\
![](<../.gitbook/assets/image (48).png>)

<img src="../.gitbook/assets/Screenshot 2024-06-22 at 21.36.53.png" alt="" data-size="original">
{% endhint %}



If you have opted for a configuration [at the component level](../customization-strategies/component-level-customization/) it can come handy to define you own custom message keys:

<pre class="language-typescript" data-title="src/login/i18n.ts"><code class="lang-typescript">import { i18nBuilder } from "keycloakify/login";
import type { ThemeName } from "../kc.gen";

/** @see: https://docs.keycloakify.dev/i18n */
const { useI18n, ofTypeI18n } = i18nBuilder
    .withThemeName&#x3C;ThemeName>()
    .withExtraLanguages({ /* ... */ })
    .withCustomTranslations({
        en: {
            loginAccountTitle: "Log in to your ACME account",
<strong>            myCustomMessage: "This is a custom message"
</strong>        },
        // cspell: disable
        fr: {
            loginAccountTitle: "Connectez-vous a votre compte ACME",
<strong>            myCustomMessage: "Ceci est un message personnalisé"
</strong>        }
        // cspell: enable
    })
    .build();

type I18n = typeof ofTypeI18n;

export { useI18n, type I18n };
</code></pre>

You'll then be able to use the message key "myCustomMessage" in your components:

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
If you are implementing theme variants you can provides translations on a per-theme variant basis.  [See how](../theme-variants.md).
{% endhint %}

Now this is perfect for defining generale purpose text. But some other translations messages are more specific to a specific Keycloak configuration and are best configured via the Keycloak Account Console.  \
I'm thinking in particular as translations related to custom user attribues (favourite pet for example) or terms and conditions.

## In the Keycloak Realm configuration

Some relevant messages, namely `termsText` and all the messages used in the User Profile Attributes like for example the Display name, the helper text or the select option labels can be defined at the realm level and it will work as you would expect:

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>The custom user attribute favourite_pet has for Display Name the message key "profile.attributes.favourite_pet"</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption><p>A translation for the message key "profile.attributes.favourite_pet" has been defined for the English language: "Favourite Pet"</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (43).png" alt="" width="312"><figcaption><p>"Favourite Pet" is correctly used as Display Name for the input field in the register page</p></figcaption></figure>

Note that if you try to use:

```tsx
msg("profile.attributes.favourite_pet");
```

It will work at runtime, you'll get `Favourite Pet` but typescript will complain because `"profile.attributes.favourite_pet"` or `string` isn't a known i18n message key, it makes sense as it's only defined on the server.

This is why you'll see in some place in the code the usage of `advancedMsg(attribute.displayName)`, `advancedMsg()` is basically equivalent to `msg()` except that TypeScript won't complain if the key isn't part of the statically defined set.\
[More details](https://github.com/keycloakify/keycloakify/blob/60aaa03202763307a82991c38997d166f8f44d65/src/login/i18n/i18n.tsx#L58-L72).\
\
See also:

{% content-ref url="../terms-and-conditions.md" %}
[terms-and-conditions.md](../terms-and-conditions.md)
{% endcontent-ref %}

[Adding registration form fields](https://www.youtube.com/watch?v=lMOLrdqilqE\&t=88s).
