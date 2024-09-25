---
description: Customizing the Single Page Account UI
---

# Single-Page

The present documation page is a transcript of what I explain in this video:

{% embed url="https://youtu.be/PCNd3Nso1mY" %}
This excerpt is from a video call with the Keycloak team, where we introduced the support for Account SPA customization.
{% endembed %}

## Initializing the Single-Page Account Theme

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption><p>The account Single Page Account theme before customization</p></figcaption></figure>

You've made your mind and opted for the Single Page Account UI?  \
Great, let's start by initializing you theme: &#x20;

```bash
npx keycloakify initialize-account-theme
```

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption><p>When prompted, select the "Single-Page" option</p></figcaption></figure>

This command will create the nessesary boilerpate and add to your project dependencies the required dependencies of the latest Account UI.

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption><p>Dependency added to your project when using initializing the Single-Page Account UI</p></figcaption></figure>

Before starting the customization, let's make sure that everything works by adding a simple console.log.

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";

const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;
    
<strong>    console.log("This is my account theme!");
</strong>    
    return &#x3C;KcAccountUiLoader kcContext={kcContext} KcAccountUi={KcAccountUi} />;
}
</code></pre>

Then let's start Keycloak and test it live:

```bash
npx keycloakify start-keycloak
```

Selet Keycloak 25.

<figure><img src="../.gitbook/assets/image (135).png" alt="" width="375"><figcaption><p>Keycloak 25 up and runing on your computer</p></figcaption></figure>

Once you get the confirmation message that Keycloak is up and running you can reach the https://my-theme.keycloakify.dev to get redirected to your Login theme.\
Use the test credentials to authenticate as the test user:

<figure><img src="../.gitbook/assets/image (136).png" alt="" width="375"><figcaption><p>Authenticating as the test user</p></figcaption></figure>

On the next page you'll be provided with a link to the Account pages:

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption><p>Authenticated as "testuser" on the my-theme.keycloakify.dev utility app</p></figcaption></figure>

You should be able to see your log statement confirming that you are indeed running your theme.

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

Compilation of your theme is running in watch mode when using the start-keycloak command, you can eddit your console.log message, save and after a few seconds reload the page, you should see the message updated.\
\
After completing the initialization process, it's a good time to commit the changes.

```bash
git add -A
git commit -am "Initialize the Single-Page Account Theme"
```

## i18n:  Internationalization and translation

To customize the text and the translations it's done by creating .properties files in **src/account/messages**.\


{% hint style="info" %}
You can find the base i18n resources in:

**node\_modules/@keycloakify/keycloak-account-ui/messages**

Don't edit theses files directly though, keep reading!
{% endhint %}

### Modifying existing texts

Let's say we want to change some text in the UI.&#x20;

Create the following file:

{% code title="src/account/messages/messages_en.properties" %}
```properties
personalInfo=Personal info cool
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

Result:

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption></figcaption></figure>

We just changed the text only in english. You need to do the same for all the languages that you wish to support.

{% hint style="success" %}
You can also change the texts in the Keycloak Account UI in the Localization tab.\
Learn more: [See this](../i18n/adding-new-translation-messages-or-changing-the-default-ones.md#in-the-keycloak-realm-configuration).
{% endhint %}

### Adding support for a new language

We just see how to add or modify texts for languages that are supported by default. &#x20;

If, however, you want to add support for an extra language, you can do it by creating a new propery file.

Example, adding support for the Vietnamese:

<figure><img src="../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

You can now go in the Keycloak Admin Console and select Vn in the list of supported language:

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption><p>Note: Keycloak does not provide a way to provide the corect label at the moment</p></figcaption></figure>

Result after selecting Vn in the Account UI:&#x20;

<figure><img src="../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

## Basic customization

You can customize some aspect of the account theme witout having to go down at the React component level.  \
If you stick to this level of customization the Keycloak team is able to guarenty that you'll be able to keep your theme compatible with upcoming version of Keycloak with minimal maintenance effort. &#x20;

Let's see what we can do.

### Changing the logo

First start by adding a logo file in your src directory somewhere, example:&#x20;

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

And pass a reference to it as props of the `<KcAccountUiLoader />` component:

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";
<strong>import myLogoPngUrl from "./assets/my-logo.png";
</strong>
const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    return (
        &#x3C;KcAccountUiLoader
            kcContext={kcContext}
            KcAccountUi={KcAccountUi}
<strong>            logoUrl={myLogoPngUrl}
</strong>        />
    );
}
</code></pre>

After saving and reloading you should be able to see that the logo has been updated:

<figure><img src="../.gitbook/assets/image (141).png" alt="" width="375"><figcaption><p>The Keycloak logo has been replaced by the Keycloakify logo</p></figcaption></figure>

### Customizing what sections are availables in the left pannel

Beside changing option in your Keycloak realm configuration you can define what options should be displayed and when in the left pannel.  \
For that you can use the content props of the  `KcAccountUiLoader` component. &#x20;

You can start by copy/pasting the default content located in **node\_modules/@keycloakify/keycloak-account-ui/src/public/content.ts**

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";
import myLogoPngUrl from "./assets/my-logo.png";

const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    return (
        &#x3C;KcAccountUiLoader
            kcContext={kcContext}
            KcAccountUi={KcAccountUi}
            logoUrl={myLogoPngUrl}
<strong>            content={[
</strong><strong>                {
</strong><strong>                    label: "personalInfo",
</strong><strong>                    path: ""
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "accountSecurity",
</strong><strong>                    children: [
</strong><strong>                        {
</strong><strong>                            label: "signingIn",
</strong><strong>                            path: "account-security/signing-in"
</strong><strong>                        },
</strong><strong>                        {
</strong><strong>                            label: "deviceActivity",
</strong><strong>                            path: "account-security/device-activity"
</strong><strong>                        },
</strong><strong>                        {
</strong><strong>                            label: "linkedAccounts",
</strong><strong>                            path: "account-security/linked-accounts",
</strong><strong>                            isVisible: "isLinkedAccountsEnabled"
</strong><strong>                        }
</strong><strong>                    ]
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "applications",
</strong><strong>                    path: "applications"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "groups",
</strong><strong>                    path: "groups",
</strong><strong>                    isVisible: "isViewGroupsEnabled"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "resources",
</strong><strong>                    path: "resources",
</strong><strong>                    isVisible: "isMyResourcesEnabled"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "oid4vci",
</strong><strong>                    path: "oid4vci",
</strong><strong>                    isVisible: "isOid4VciEnabled"
</strong><strong>                }
</strong><strong>            ]}
</strong>        />
    );
}
</code></pre>

Let's for example comment out the deviceActivity section.

<figure><img src="../.gitbook/assets/image (142).png" alt="" width="297"><figcaption><p>Before</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (143).png" alt="" width="375"><figcaption><p>Commenting out deviceActivity</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (144).png" alt="" width="315"><figcaption><p>After, the Device Activity section has disapeared</p></figcaption></figure>

### Adding custom CSS

The official way of customizing the look of the Account UI is to overload the PaternFly CSS variables:&#x20;

{% embed url="https://www.patternfly.org/components/button/html/#css-variables" %}

But we are aware that this isn't enough for most of you. &#x20;

Beyond that you can of course import your custom CSS and use the PaternFly utility classes as target but be aware that theses classes can be changed in the future.

\
Example loading some custom CSS:

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption><p>The red boder has been applied on all the element that have the pf-v5-c-page__main-secrion class</p></figcaption></figure>

## Component level customization

To customize the Account theme at the React component level you want to use the eject-page command and slect "account".

```bash
npx keycloakify eject-page
```

<figure><img src="../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

After running this command you'll be able to see that the following change has been automatically applied:

{% code title="src/account/KcPage.tsx" %}
```diff
 import { lazy } from "react";
 import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
 import type { KcContext } from "./KcContext";

-const KcAccountUi = lazy(()=> import("@keycloakify/keycloak-account-ui/KcAccountUi"));
+const KcAccountUi = lazy(() => import("./KcAccountUi"));

 export default function KcPage(props: { kcContext: KcContext }) {
     const { kcContext } = props;

     return (
         <KcAccountUiLoader
             kcContext={kcContext}
             KcAccountUi={KcAccountUi}
         />
     );
 }
```
{% endcode %}

Also the KcAccountUi component has be copied over from **node\_modules/@keycloakify/keycloak-account-ui/src/KcAccountUi.tsx** to your codebase at **src/account/KcAccountUi.tsx**. &#x20;

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption><p>KcAccountUi.tsx has been ejected</p></figcaption></figure>

That what you'll want to each time you'll want to take ownership of some component of the Account UI: Copy the original source file into your codebase and update the absolute imports by relative imports.  \
Let's see in practice how we would eject the routes:

```bash
cp node_modules/@keycloakify/keycloak-account-ui/src/routes.tsx src/account/
```

Then you want to update the absolututes import of the routes to your local routes in KcAccountUI.tsx

```diff
- import { routes } from "@keycloakify/keycloak-account-ui/routes";
+ import { router } from "./routes";
```

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

And that's it, you now own the routes!  \
Moving forward you can incrementally "eject" the components you need to take ownerhip over by followind the same process. &#x20;

Be aware: The more components you eject, the more work it will represent to mainain your theme up to date with the future evolution of Keycloak.  &#x20;

## Updating keycloak-account-ui to a new version

Each time a new version of Keycloak is released, a new version of @keycloak/keycloak-account-ui is also released with the same version number.\
This does not nessesarely means that in order to have a custom theme that works with Keycloak 25.0.2 for example you need to use @keycloak/keycloak-account-ui@25.0.2. Actually it's very likely that you theme will still work with Keycloak 26, however at some point your theme will break with future version of Keycloak and you'll have to upgrade. &#x20;

Keycloakify does not use the NPM package @keycloak/keycloak-account-ui but an automatically repackaged distribution of it: @keycloakify/keycloak-account-ui.

So, when comes the time to upgrade you want to navigate to:\


{% embed url="https://github.com/keycloakify/keycloak-account-ui" %}

And look in the README in the installation section:

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption><p>Instalation section of keycloakify/keycloak-account-ui version 25.0.2</p></figcaption></figure>

You want to copy and paste the dependencies into the package.json of your Keycloakify project.

The readme is generated automatically, you can trust that is always up do date.

You migh wonder why there's only RC releases of @keycloakify/keycloak-account-ui, it's because we want to match the version number of the upstream package @keycloak/keycloak-account-ui but still be able to publish update when minor changes on the re-packaging distribution is needed. &#x20;

##
