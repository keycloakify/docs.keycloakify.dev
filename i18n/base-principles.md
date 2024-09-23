# Base principles



In the Keycloak Admin Console you can enable localisation by selecting a set of language that you wish to support: &#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The language that you want to support isn't in the default set? You can add support for it with Keycloakify. [See how](adding-support-for-extra-languages.md).
{% endhint %}

When Internationalization is enabled you will see a language dropdown select in your UIs:

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="success" %}
You shouldn't rely on the language select to let your users select their language. &#x20;

Infact, I encourage you to hide or remove it. &#x20;

What you should do instead is, when redirecting your user from your application to your Keycloak login page, add an extra query param to let Keycloak know in what language the page should be rendered. &#x20;

The parameter to add is ?ui\_locales=fr (Example if we want the UI to be in French). &#x20;

See [oidc-spa documentation](https://docs.oidc-spa.dev/documentation/usage) for more info on how to provide this parameter. (You can do the same if you use keycloak-js or NextAuth)
{% endhint %}



If you [eject some pages](../customization-strategies/component-level-customization/), you'll see in your component how the internationalization is actually implemented:

<pre class="language-tsx" data-title="src/login/Register.tsx"><code class="lang-tsx">export default function Register(props: RegisterProps) {
    const { i18n } = props;
    
    const { msg, msgStr, advancedMsg, advancedMsgStr } = i18n;

    return (
        //...
        &#x3C;a href={url.loginUrl}>
<strong>            {msg("backToLogin")}
</strong>        &#x3C;/a>
        // ...
    );
}
</code></pre>

<figure><img src="../.gitbook/assets/image (107).png" alt="" width="374"><figcaption><p><code>msg("backToLogin")</code> gets rendered as <strong>« Back to Login</strong></p></figcaption></figure>

If you want to see the base message translations you can navigate to the **node\_modules/keycloakify/src/login/i18n/messages\_defaultSet/** directory:

{% hint style="danger" %}
Don't edit this file directly, it's just for seeing what are the default set of i18n messages.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2024-07-14 at 18.08.03.png" alt=""><figcaption></figcaption></figure>

As you can see, the translation message for the key `backToLogin` in English (**en.ts**) is:

`We are <strong>sorry</strong> ...`

\
As a result calling `msg("backToLogin")` returns the following **JSX.Eement**:

```html
<div data-kc-msg="backToLogin">We are <strong>sorry</strong> ...</div>
```

{% hint style="info" %}
The `data-kc-msg` attribute is only here to help you find the source code that generates this node when inspecting with the browser dev tools.\
Note also that the text that is going to be rendered is:

We are **sorry** ... (with sorry in bold)
{% endhint %}

The msg() function does not return a string but a JSX.Element, if you need the literal html string, you can use the msgStr() function instead:

Calling `msgStr("backToLogin")` returns the **string**:&#x20;

```javascript
"We are <strong>sorry</strong> ..."
```

Now that you get the main idea, let's see how to add support for a new language:

{% content-ref url="adding-support-for-extra-languages.md" %}
[adding-support-for-extra-languages.md](adding-support-for-extra-languages.md)
{% endcontent-ref %}
