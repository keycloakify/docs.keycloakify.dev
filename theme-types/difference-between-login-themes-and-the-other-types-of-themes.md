---
icon: album-collection
---

# Differences Between Login Themes and Other Types of Themes

Here are the types of themes that exist:

* **Login Theme**: The UI for login and registration pages, displayed to users when they attempt to sign in or sign up.
* **Account Theme**: The account management interface, where users can update their email, change their password, and manage other account settings.
* **Email Theme**: Templates for automated emails (e.g., email confirmations or password reset notifications).
* **Admin Theme**: The Admin Console interface used by administrators to configure Keycloak.

Most of this documentation focuses on the Login Theme.

If you choose to implement a **Multi-Page Account Theme**, note that it works exactly like the Login Theme.

Here are the features that apply to all theme types:

* ✅ [Testing your theme inside Keycloak](../testing-your-theme/inside-of-keycloak.md)
* ✅ [Theme Variants](../features/theme-variants.md)
* ✅ [Environment Variables](../features/compiler-options/environmentvariables.md) (except email theme)

Below are the documentation pages that apply **only** to the Login Theme **and** the Multi-Page Account Theme but are handled differently in the other types of themes: &#x20;

* ❌ [Testing your theme outside of Keycloak](../testing-your-theme/outside-of-keycloak.md) (using `npx keycloakify add-story` and Storybook)
* ❌ [`npx keycloakify eject-page`](../common-use-case-examples/using-a-component-library.md)
* ❌ [Internationalization](../features/i18n/) — the other theme types handle translations differently.

In this section, you’ll find all the information you need to create the other types of themes with Keycloakify:

{% content-ref url="account-theme/" %}
[account-theme](account-theme/)
{% endcontent-ref %}

{% content-ref url="email-theme.md" %}
[email-theme.md](email-theme.md)
{% endcontent-ref %}

{% content-ref url="admin-theme.md" %}
[admin-theme.md](admin-theme.md)
{% endcontent-ref %}
