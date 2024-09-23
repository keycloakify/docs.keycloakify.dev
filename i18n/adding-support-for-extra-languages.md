# Adding Support for Extra Languages

Keycloak, out of the box come with availaible translation for the following languages: &#x20;

1. ar - Arabic
2. ca - Catalan
3. cs - Czech
4. da - Danish
5. de - German
6. el - Greek
7. en - English
8. es - Spanish
9. fa - Persian (Farsi)
10. fi - Finnish
11. fr - French
12. hu - Hungarian
13. it - Italian
14. ja - Japanese
15. ka - Georgian
16. lt - Lithuanian
17. lv - Latvian
18. nl - Dutch
19. no - Norwegian
20. pl - Polish
21. pt-BR - Portuguese (Brazilian)
22. pt - Portuguese
23. ru - Russian
24. sk - Slovak
25. sv - Swedish
26. th - Thai
27. tr - Turkish
28. uk - Ukrainian
29. zh-CN - Chinese (Simplified)
30. zh-TW - Chinese (Traditional)

{% hint style="success" %}
If the languages you with to support is included in this list, great, you can [skip to the next section](previewing-you-pages-in-different-languages.md) otherwise keep reading.
{% endhint %}

Let's see how to add support for an extra language so you can enable it in the Keycloak Admin UI.

In this example we're going to add Hindi (hi).

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Hindi appear as a supported locales option in the Keycloak Account UI</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="375"><figcaption><p>Hindi appear as an option in the language select dropdown</p></figcaption></figure>

To acheive this first step is to create a TypeScript file containing Hindi translation of all the default message keys. I sugest creating this file as `src/login/i18n.hi.ts` but it can be located anywhere under your src directory.

{% code title="src/login/i18n.hi.ts" lineNumbers="true" %}
```typescript
import type { MessageKey_defaultSet } from "keycloakify/login";

const messages: Record<MessageKey_defaultSet, string> = {
    // cspell: disable
    doLogIn: "साइन इन करें",
    doRegister: "रजिस्टर करें",
    doRegisterSecurityKey: "रजिस्टर करें",
    // ... translation for all the other default messages
    // cspell: enable
};

export default messages;
```
{% endcode %}

Wait before panicking realizing that there are hundreds of message keys. ChatGPT can do this for you!

Just take the English translation of the message a reference and ask it to translate for you (you'll have to press continue several times). &#x20;

You can find the English translations in your repo at:&#x20;

**node\_modules/keycloakify/src/login/i18n/messages\_defaultSet/en.ts**

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**Do not customize the translations messages to fit your usecase**!&#x20;

This is not the place to do it. \
Theses translation should be as generic as they can be.

This process is just a temporary sollution until Keycloak add support for your language, you should be prepared to to delete the file as soon as it's no longer nessesary.  \
For customizing the translation messages and adding your custom messages see [this section](adding-new-translation-messages-or-changing-the-default-ones.md).
{% endhint %}

Next step is to index the translation file that you have created, edit the **src/login/i18n.ts** file as follow:

<pre class="language-typescript" data-title="src/login/i18n.ts"><code class="lang-typescript">import { i18nBuilder } from "keycloakify/login";
import type { ThemeName } from "../kc.gen";

/** @see: https://docs.keycloakify.dev/i18n */
const { useI18n, ofTypeI18n } = i18nBuilder
    .withThemeName&#x3C;ThemeName>()
<strong>    .withExtraLanguages({
</strong><strong>        hi: {
</strong><strong>            // cspell: disable-next-line
</strong><strong>            label: "हिन्दी",
</strong><strong>            getMessages: () => import("./i18n.hi")
</strong><strong>        }
</strong><strong>    })
</strong>    .build();

type I18n = typeof ofTypeI18n;

export { useI18n, type I18n };
</code></pre>

That's it! \
Now, you should be able to enable Hindi in the Account Console of the Keycloak where your theme is loaded.

Now let's see how to preview the your pages in different languages during devlopement.

{% content-ref url="previewing-you-pages-in-different-languages.md" %}
[previewing-you-pages-in-different-languages.md](previewing-you-pages-in-different-languages.md)
{% endcontent-ref %}
