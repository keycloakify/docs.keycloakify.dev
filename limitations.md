# ⚠️ Limitations

### Some pages still have the default theme. Why?

This project only support out of the box the most common user facing pages of Keycloak but not all of them. You might find out that a page that you need to customize is not yet provided as a react component by Keycloakify. &#x20;

[Here](https://github.com/InseeFrLab/keycloakify/tree/main/src/login/pages) are the login pages currently implemented vs [all the existing pages](https://github.com/keycloak/keycloak/tree/main/themes/src/main/resources/theme/base/login). &#x20;

[Here](https://github.com/InseeFrLab/keycloakify/tree/main/src/account/pages) are the acount pages currently implemented vs [all the existing pages](https://github.com/keycloak/keycloak/tree/21.1.2/themes/src/main/resources/theme/base/account).  \
\
You can preview the pages looks in [the Storybook](https://storybook.keycloakify.dev/?path=/story/introduction--page). (Well of course the pages that are not yet implemented will not apprear here.) &#x20;

If the page you are looking for is implemented you can import it in you project with this command: &#x20;

`npx -p keycloakify download-builtin-keycloak-theme`&#x20;

[Video demo](https://user-images.githubusercontent.com/6702424/164304458-934b0e1d-9de7-4bb4-8a1c-e06a70b1636a.mov)  \
\
(Before copying the page in your project try to see if you can't acheive the desired look by customizing the Template.tsx)\
\
Otherwise, keep reading.

#### I have established that a page that I need isn't supported out of the box by Keycloakify, now what? &#x20;

{% tabs %}
{% tab title="I submit a PR to Keycloakify" %}
{% content-ref url="contributing.md" %}
[contributing.md](contributing.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="I do it only for my project" %}
Keycloakify also enables you to declare custom ftl pages. &#x20;

Check out how `my-extra-page-1.ftl` and `my-extra-page-2.ftl` where added to [the demo project](https://github.com/garronej/keycloakify-demo-app/tree/look\_and\_feel). &#x20;

Main takeaways are:

* (TS only) You must declare theses page in the type argument of the getter function for the `kcContext` in order to have the correct typings. [example](https://github.com/garronej/keycloakify-demo-app/blob/4eb2a9f63e9823e653b2d439495bda55e5ecc134/src/KcApp/kcContext.ts#L16-L21)
* (TS only) If you use Keycloak plugins that defines non standard `.ftl` values (Like for example [this plugin](https://github.com/micedre/keycloak-mail-whitelisting) that define `authorizedMailDomains` in `register.ftl`) you should declare theses value to get the type. [example](https://github.com/garronej/keycloakify-demo-app/blob/4eb2a9f63e9823e653b2d439495bda55e5ecc134/src/KcApp/kcContext.ts#L6-L13)
* You should provide sample data for all the non standard value if you want to be able to debug the page outside of keycloak. [example](https://github.com/garronej/keycloakify-demo-app/blob/4eb2a9f63e9823e653b2d439495bda55e5ecc134/src/KcApp/kcContext.ts#L28-L43)
{% endtab %}
{% endtabs %}

### `process.env.PUBLIC_URL`  not supported.

The environement variable process.env.PUBLIC\_URL can't be directly used in a Keycloakify project but there is a workaround. See: &#x20;

[#import-assets-from-the-public-directory](importing-assets.md#import-assets-from-the-public-directory "mention")

In Vite `import.meta.env.BASE_URL` is supported!&#x20;

### Self hosted fonts

Importing self hosted font does not work out of the box like it would in a regular React project.  \
See workaround: &#x20;

[#selft-hosted-fonts](importing-assets.md#selft-hosted-fonts "mention")

#### Other workarounds

* You can circumvent the problem by avoiding hosting your font yourself using Google Fonts, Font Awesome  or any other font provider.
* You can [self host your font somewhere](https://github.com/garronej/keycloakify-demo-app/blob/2de8a9eb6f5de9c94f9cd3991faad0377e63268c/src/fonts.scss#L16), you will need to enable [`Access-Control-Allow-Origin`](https://github.com/garronej/keycloakify-demo-app/blob/2de8a9eb6f5de9c94f9cd3991faad0377e63268c/nginx.conf#L17-L19) on the server serving your fonts.

### Admin Console Theme not supported

<figure><img src=".gitbook/assets/Admin_console_theme_not_supported.png" alt=""><figcaption></figcaption></figure>

If you are missing this feature [open an issue about it](https://github.com/InseeFrLab/keycloakify/issues/new).

### Wellcome page not customizable

The following page isn't customizable yet.

<figure><img src=".gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

It's however planned to enable this. Follow the progress in [this issue](https://github.com/keycloakify/keycloakify/issues/148).

### Field Names can't be runtime generated

Keycloakify analyze your code to see what field name are used. As a result your field names should be hard coded in your code (If you are [using user profile](realtime-input-validation.md) you don't have to worry about it). &#x20;

```jsx
// OK ✅
messagesPerField.exists("foo-bar")

// Not OK 🛑
const bar= "bar";
messagesPerField.exists(`foo-${bar}`);
```

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-limitations" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud-IAM consulting services to simplify your experience.
{% endembed %}
