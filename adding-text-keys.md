# 🌎 Internationalization and Translations

Internaltionalization and Translation, refered as i18n is the set of feature that enables you to make your pages available in multiple languages. &#x20;

## Base principles

When in your components you see instructions like:

<pre class="language-tsx" data-title="src/login/Register.tsx"><code class="lang-tsx">export default function Register(props: RegisterProps) {
    const { i18n } = props;
    
    const { msg, msgStr } = i18n;

    return (
        //...
        &#x3C;a href={url.loginUrl}>
<strong>            {msg("backToLogin")}
</strong>        &#x3C;/a>
        // ...
    );
}
</code></pre>

`msg("backToLogin")` returns a JSX.Element, the value of the the translation message backToLogin in the current language (`kcContext.locale?.currentLanguageTag`) rendered as an HTML string.

If you use `msgStr("backToLogin")` you get the raw string. &#x20;

There is also the `advancedMsg()` and `advancedMsgStr()` functions that are to be used when the message key is a dynamical variable or when you're unsure if the translation for a given key exist or not. Learn more about advancedMsg [here](https://github.com/keycloakify/keycloakify/blob/9f1186302ea8d65caf5e1a220ba1635213de7293/src/login/i18n/i18n.tsx#L55-L82).

## Overriding the bases message or adding cusom ones

### In the theme

If you want to see the base message tranlations you can navigate to the **node\_modules/keycloakify/src/login/i18n/baseMessages/** directory: &#x20;

<figure><img src=".gitbook/assets/Screenshot 2024-06-22 at 20.37.14.png" alt=""><figcaption></figcaption></figure>

As you can see, the translation message for the key backToLogin in English (**en.ts**) is:

&#x20;`&laquo; Back to Login`

\&laquo; is the representation of the '**«'** character in HTML notation.  \
As a result: &#x20;

* `msg("backToLogin")` will return the `JSX.Element`: `<span>« Back to Login</span>`
* `msgStr("backToLogin")` will return the `string`: `"&laquo; Back to Login"`

Keycloakify let's you overwite the values of the translations messages and define new ones.\
This is how to do it:

{% code title="src/login/i18n.tsx" %}
```tsx
import { createUseI18n } from "keycloakify/login";

export const { useI18n, ofTypeI18n } = createUseI18n({
    en: {
        backToLogin: "⏪ Back to <strong>Login page</strong>",
        myCustomKey: "My custom message"
    },
    fr: {
        backToLogin: "⏪ Retour à la <strong>page de Login</strong>",
        myCustomKey: "Mon message personalisé"
    }
});

export type I18n = typeof ofTypeI18n;
```
{% endcode %}

{% hint style="warning" %}
The messageBundle that you provide as argument of the `createUseI18n` function must be statically evaluable. You can't import from external file. All the translations must be declared inline.  \
This is because Keycloakify will analyse your code at build time to make Keycloak aware of your modifications of the base messages so that server side generated feedback messages can use your translations. &#x20;

![](<.gitbook/assets/image (3).png>)\
![](<.gitbook/assets/image (4).png>)

![](<.gitbook/assets/Screenshot 2024-06-22 at 21.36.53.png>)


{% endhint %}

If you don't provide tranlation for all the language that are enabled in the realm configuration it will fallback to english (or your first tranlation declared). &#x20;

### In the Keycloak Realm configuration



\
