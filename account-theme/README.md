# 👤 Account Theme

## Deciding if you need an account theme or not

The first question you want to ask yourself is: "Do I really need an account theme?" &#x20;

If you're looking to create an account theme just to allow your users to change their password, update account information such as email, phone number, favorite pets, etc., or delete their account, then you do **not** need an account theme.

There are pages in the login theme for those functions. You only need to add a button to redirect your user to the appropriate page in your main app. Here is how to do it with oidc-spa: [Documentation](https://docs.oidc-spa.dev/documentation/user-account-management).

An added benefit of not having an account theme is that you will reuse the exact same form that you created for the registration page in the `login-update-profile.ftl` page.&#x20;

Consider creating an account theme only if you need to provide advanced account management features to your users, such as connection logs, management of active sessions, file uploads, etc.&#x20;

Here is a video that explains this in detail:

{% embed url="https://youtu.be/PiTUPdpmueA" %}

## Choosing an account theme type

Keycloakify provide you two ways to create your own account theme. &#x20;

You can chose between two implementation of the account theme:

### Single Page

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Screenshot of the Single Page Account theme</p></figcaption></figure>

The Single Page theme also refered as account v3 is this the default theme that comes with Keycloak 25.

#### Pros

* Get's all the latest features.
* The base code is maintained by the Keycloak team and automatically integrated into Keycloakify. You're using the real thing, not a fork.
* If you're a React developper you'll feel right at home. It's uing i18n-next and react-router

#### Cons

* Opting for this option will add a lot of dependencies to your project (i18n-next, react-router-dom, patenrnfly and more).
* CSS level customization beyond [overidding the Paternfly CSS variables](https://www.patternfly.org/components/button/html/#css-variables) is not practical, you'll have to customize at the React component level.
* No Storybook support.
* No `npx keycloakify eject-page` CLI, you'll have to manually copy paste from the source the components you want to take ownership over.
* Not currently supported by Keycloak 21 (Specifically this version). If you want me to add support for it [open an issue](https://github.com/keycloakify/keycloakify/issues/new).
* When upgrading to a future version of Keycloak, there’s a possibility that your account theme may break. If your customizations are limited to styles, updating should be straightforward—simply bump the version number of [the Account UI](https://github.com/keycloakify/keycloak-account-ui) in your dependencies. However, if you’ve made customizations at the React component level, migrating to the new version could require substantial effort due to potential extensive changes in the underlying code. The Keycloakify team cannot guarantee a specific level of stability for these modifications, as they are not part of our codebase.  &#x20;

To get started with the Single-Page account theme:

{% content-ref url="single-page.md" %}
[single-page.md](single-page.md)
{% endcontent-ref %}

### Multi Page

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This is a fork of the Account v1 maintained by the Keycloakify team.

#### Pros

* Works exactly the same as the login theme, nothing new to learn.
* Storybook support
* CSS level customization support just like in the login theme.
* As it's maintained by us, we can guarenty a certain level of stability in future version of Keycloak.
* Compatible with all Keycloak version.
* Does not add any dependency to your project.

#### Cons

* Don't come with all the feature out of the box yet. You'll have to use the [Keycloak Account REST API if you want to implement them](multi-page.md). &#x20;
* It relies on Java code maintained by us, this code uses Keycloak internal API, you have to trust us to keep maintaining it.
* The default look is a bit dated (as of today, we'll update it). &#x20;

To get started with the Multi-Page account theme:

{% content-ref url="multi-page.md" %}
[multi-page.md](multi-page.md)
{% endcontent-ref %}
