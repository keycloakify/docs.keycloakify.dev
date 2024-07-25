# 👤 Account Theme

Keycloakify provide you two ways to create your own account theme. &#x20;

You can chose between two implementation of the account theme:

## Single Page

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Screenshot of the Single Page Account theme</p></figcaption></figure>

The Single Page theme also refered as account v3 is this the default theme that comes with Keycloak.

### Pros

* Get's all the latest features.
* The base code is maintained by the Keycloak team and automatically integrated into Keycloakify. You're using the real thing, not a fork.
* If you're a React developper you'll feel right at home. It's uing i18n-next and react-router

### Cons

* Opting for this option will add a lot of dependencies to your project (i18n-next, react-router-dom, patenrnfly and more).
* CSS level customization beyond overloading of Paternlyfly CSS variable is not practical, you'll have to customize at the React component level.
* No Storybook support.
* Not backward compatible with older Keycloak version prior 25
* If you go beyond surface level customization, keeping the theme up to date as new Keycloak version get released can be challenging.  &#x20;

## Multi Page

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

This is a fork of the Account v1 maintained by the Keycloakify team

### Pros

* Works exactly the same as the login theme, nothing new to learn.
* Storybook support
* CSS level customization support just like in the login theme.
* As it's maintained by us, we can guarenty a certain level of stability in future version of Keycloak.
* Compatible with all Keycloak version.
* Does not add any dependency to your project.

### Cons

* Don't come with all the feature out of the box yet. You'll have to use the [Keycloak Account REST API if you want to implement them](account-rest-api.md). &#x20;
* Uses internal Keycloak API and custom Java code, you have to trust us to keep maintaining it.
* The default looks is a bit dated (as of today, we plan to update it in the future). &#x20;

## Command for initializing the theme

When you've made your choice, you can initialize the account theme with:

```bash
npx keycloakify initialize-account-theme
```
