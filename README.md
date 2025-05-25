---
icon: rocket-launch
---

# Quick Start

{% embed url="https://www.youtube.com/watch?v=S-B8Db2dStk" %}
Introduction and Tutorial
{% endembed %}

**Keycloakify** is a tool for creating custom Keycloak themes, enabling you to modify the appearance and behavior of Keycloak's user interfaces. This includes:

* **Login Theme**: The UI for login and registration pages, displayed to users when they attempt to log in or sign up.
* **Account Theme**: The account management interface, where users can update their email, change their password, and manage other account settings.
* **Email Theme**: The templates used by Keycloak for automated emails, such as email confirmation or password reset notifications.
* **Admin Theme**: The Admin Console interface, used by administrators to configure Keycloak.

For a visual preview of these UIs as they appear with Keycloak's built-in theme, visit the Storybook demonstration:

{% embed url="https://storybook.keycloakify.dev/" %}

Whether you need to apply light CSS-level customizations to the built-in UIs or perform deeper redesigns at the component level using React, Angular, or Svelte, Keycloakify is the tool to achieve your goals.\
\
If you’d like to see Keycloakify in action, check out [Neon](https://neon.tech/). Clicking **Login** or **Sign Up** will take you to their Keycloakify-themed login pages.

## Why Choose Keycloakify?

You might be wondering why you would need a third-party tool like Keycloakify to create your custom UIs instead of relying solely on [Keycloak's built-in theming system](https://www.keycloak.org/docs/latest/server_development/#_themes). Here are a few reasons:

* **Leverage Modern Frontend Technologies**: Keycloakify enables you to use TypeScript, React, Angular, Svelte, and any styling solution or component library you prefer, such as Tailwind, MUI, shadcn/ui, or plain CSS.
* **Streamlined Testing**: Keycloakify makes it easy to [test your theme](testing-your-theme/) both [inside](testing-your-theme/inside-of-keycloak.md) and [outside](testing-your-theme/outside-of-keycloak.md) Keycloak, with hot reloading for a smoother development experience.
* **Automated Theme Bundling**: Keycloakify [bundles your theme into a JAR file](deploying-your-theme.md#building-the-jar-file), ready to import directly into Keycloak.
* **Version Compatibility**: Themes generated with Keycloakify are backward compatible with Keycloak versions as far back as 11 and are [designed to remain compatible with future Keycloak updates](#user-content-fn-1)[^1].
* **Built-In Real-Time Validation**: Keycloakify includes real-time frontend validation by default. For example, users receive instant feedback, such as "The password must be at least 12 characters long," rather than waiting until they press the submit button.
* **Community Support**: We're here to help! If you're stuck or need guidance, reach out through our [Discord channel](https://discord.gg/kYFZG7fQmn) or [GitHub issues](https://github.com/keycloakify/keycloakify/issues/new). We respond quickly and are happy to assist.

If you’re still unsure or want a better understanding before committing to using Keycloakify, check out this guide:

{% content-ref url="https://app.gitbook.com/s/H3WkQf6kDTNqLCk7O5D7/how-it-works" %}
[https://app.gitbook.com/s/H3WkQf6kDTNqLCk7O5D7/how-it-works](https://app.gitbook.com/s/H3WkQf6kDTNqLCk7O5D7/how-it-works)
{% endcontent-ref %}

## Pick Your Framework: React, Angular or Svelte

Keycloakify supports React, Angular, and Svelte, allowing you to work with the framework you're most familiar with. If you're only making CSS-level customizations to Keycloak's built-in theme, any of these frameworks will work. For a smoother experience, **React** is recommended, as it has the most complete integration.

For Angular and Svelte users, a few considerations apply:

* **Account Themes**: [The starting UI differs from Keycloak's default](#user-content-fn-2)[^2], requiring additional adjustments.
* **Admin Themes**: Only React supports custom Admin UIs. However, since the Admin UI is only seen by the Keycloak instance administrator, it is rarely customized.
* **Angular Setup**: [Keycloakify cannot be directly installed in an existing Angular project](#user-content-fn-3)[^3]. Themes must either be standalone projects or a subproject in a monorepo.

React provides the most seamless experience, but Angular and Svelte are fully supported for login and account themes with some extra effort, which covers the needs of most projects. Choose the framework that best suits your project and expertise. If you have any questions or concerns, feel free to ask us on our [Discord channel](https://discord.gg/kYFZG7fQmn).

## Quick Start

Before futher reading, some practice!

{% tabs %}
{% tab title="React" %}
```bash
git clone https://github.com/keycloakify/keycloakify-starter
```
{% endtab %}

{% tab title="Angular" %}
```bash
git clone https://github.com/keycloakify/keycloakify-starter-angular-vite keycloakify-starter
```

Credit goes to [@kathari00](https://github.com/kathari00) for taking the initiative and driving the development of Angular support.
{% endtab %}

{% tab title="Svelte" %}
```bash
git clone https://github.com/keycloakify/keycloakify-starter-svelte keycloakify-starter
```

Credit goes to [@luca-peruzzo](https://github.com/luca-peruzzo) for taking the initiative and driving the development of Svelte support.
{% endtab %}
{% endtabs %}

Let's create a story for the Login page and run Storybook[^4].

```bash
cd keycloakify-starter
yarn install # Feel free to use another package manager (if you do, remove the .yarn.lock)
npx keycloakify add-story # Select login.ftl (for example)
npm run storybook 
```

You should now be able to see the login pages in different scenarios:

<figure><img src=".gitbook/assets/grafik (2).png" alt=""><figcaption><p>View of the storybook interface that let you preview the login page of your theme.</p></figcaption></figure>

Now, let's apply our first CSS customization.

Create the following stylesheet:

{% code title="src/login/main.css" %}
```css
.kcFormHeaderClass {
    border: 3px solid red;
}
```
{% endcode %}

And import it:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx"><strong>import "./main.css";
</strong>// ...
</code></pre>
{% endtab %}

{% tab title="Angular" %}
<pre class="language-typescript" data-title="src/login/KcPage.ts"><code class="lang-typescript"><strong>import "./main.css";
</strong>import { getDefaultPageComponent, type KcPage } from '@keycloakify/angular/login';
// ...
</code></pre>
{% endtab %}

{% tab title="Svelte" %}
<pre class="language-html" data-title="src/login/KcPage.svelte"><code class="lang-html">&#x3C;script lang="ts">
<strong>  import "./main.css";
</strong>  import Template from '@keycloakify/svelte/login/Template.svelte';
  ...
</code></pre>
{% endtab %}
{% endtabs %}

This is what you should be getting:

<figure><img src=".gitbook/assets/grafik (3).png" alt=""><figcaption><p>Screenshot showing the red border being applied to the header section of the login card.</p></figcaption></figure>

Now let's see how CSS customization works in Keycloakify:

{% content-ref url="css-customization.md" %}
[css-customization.md](css-customization.md)
{% endcontent-ref %}

[^1]: Of course, we can't guarantee that a given build of your theme will work with future major versions of Keycloak. However, our goal is to ensure that when a new Keycloak version introduces breaking changes, the only action required to make your theme compatible is updating the Keycloakify version and rebuilding your theme—no additional adjustments needed.

[^2]: The difference exists because recent Keycloak versions use a React-based Account UI by default, which has not been ported to Angular or Svelte in Keycloakify due to the complexity involved. For Angular and Svelte projects, Keycloakify provides a base UI derived from an older version of the Keycloak Account UI, used before the switch to React.

[^3]: Keycloakify cannot be integrated directly into standard Angular projects because the Keycloakify compiler is designed to work with Vite and Webpack. However, most Angular projects today use Esbuild by default, which is not currently supported.

[^4]: [Storybook](https://storybook.js.org/) is a tool that allows you to develop UI components in isolation. It is set up in the Keycloakify starter repos because it provides an good developer experience and makes it easy to quickly preview the different pages of your theme.
