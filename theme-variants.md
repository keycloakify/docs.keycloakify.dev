# 🎭 Theme Variants

Theme variant enables you to create multiples Keycloak theme with a single codebase.

{% tabs %}
{% tab title="Vite" %}
{% code title="vite.config.ts" %}
```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(),
    keycloakify({
      themeName: ["keycloakify-starter", "keycloakify-starter-variant-1"],
    }),
  ],
});
```
{% endcode %}
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
  "keycloakify": {
    "themeName": ["keycloakify-starter", "keycloakify-starter-variant-1"]
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

In your code you'll be able to load different styles based on the value of `kcContext.themeName`:

<figure><img src=".gitbook/assets/image (34).png" alt=""><figcaption><p>NOTE: You need to <code>run npm run dev</code>, <code>npm run storybook</code> or <code>npm run build-keycloak-theme</code> for the types to be updated.</p></figcaption></figure>

{% embed url="https://youtu.be/Nkoz1iD-HOA" %}
Tutorial video
{% endembed %}

## Different text for each of your theme variants

Keycloakify lets you provide custom tranlations on a per-theme variant basis.

{% hint style="info" %}
Read [this](i18n/adding-new-translation-messages-or-changing-the-default-ones.md) first for context.
{% endhint %}

Example:

<pre class="language-typescript"><code class="lang-typescript">import { i18nBuilder } from "keycloakify/login";
import type { ThemeName } from "../kc.gen";

/** @see: https://docs.keycloakify.dev/i18n */
const { useI18n, ofTypeI18n } = i18nBuilder
    .withThemeName&#x3C;ThemeName>()
    .withExtraLanguages({ /* ... */ })
    .withCustomTranslations({
        en: {
            doLogIn: "Log in!",
<strong>            loginAccountTitle: {
</strong><strong>                "my-theme-1": "Log in to your ACME1 account",
</strong><strong>                "my-theme-2": "Log in to your ACME2 account"
</strong><strong>            }
</strong>        },
        // cspell: disable
        fr: {
            doLogIn: "Se connecter!",
<strong>            loginAccountTitle: {
</strong><strong>                "my-theme-1": "Connectez-vous à votre compte ACME1",
</strong><strong>                "my-theme-2": "Connectez-vous à votre compte ACME2"
</strong><strong>            }
</strong>        }
        // cspell: enable
    })
    .build();

type I18n = typeof ofTypeI18n;

export { useI18n, type I18n };

</code></pre>

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>"my-theme-1" view</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>"my-theme-2" view</p></figcaption></figure>
