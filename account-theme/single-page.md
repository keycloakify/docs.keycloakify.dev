---
description: Customizing the Single Page Account UI
---

# Single-Page

## Initializing the Single-Page Account Theme

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption><p>The account Single Page Account theme before customization</p></figcaption></figure>

You've made your mind and opted for the Single Page Account UI?  \
Great, let's start by initializing you theme: &#x20;

```bash
npx keycloakify initialize-account-theme
```

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption><p>When prompted, select the "Single-Page" option</p></figcaption></figure>

This command will create the nessesary boilerpate and add to your project dependencies the required dependencies of the latest Account UI.

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption><p>Dependency added to your project when using initializing the Single-Page Account UI</p></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (69).png" alt="" width="375"><figcaption><p>Keycloak 25 up and runing on your computer</p></figcaption></figure>

Once you get the confirmation message that Keycloak is up and running you can reach the https://my-theme.keycloakify.dev to get redirected to your Login theme.\
Use the test credentials to authenticate as the test user:

<figure><img src="../.gitbook/assets/image (70).png" alt="" width="375"><figcaption><p>Authenticating as the test user</p></figcaption></figure>

On the next page you'll be provided with a link to the Account pages:

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption><p>Authenticated as "testuser" on the my-theme.keycloakify.dev utility app</p></figcaption></figure>

You should be able to see your log statement ([printed twice](#user-content-fn-1)[^1]), confirming that you are indeed running your theme.

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

Compilation of your theme is running in watch mode when using the start-keycloak command, you can eddit your console.log message, save and after a few seconds reload the page, you should see the message updated.\
\
After completing the initialization process, it's a good time to commit the changes.

```bash
git add -A
git commit -am "Initialize the Single-Page Account Theme"
```

## Basic customization

You can customize some aspect of the account theme witout having to go down at the React component level.  \
If you stick to this level of customization the Keycloak team is able to guarenty that you'll be able to keep your theme compatible with upcoming version of Keycloak with minimal maintenance effort. &#x20;

Let's see what we can do.

### Changing the logo

First start by adding a logo file in your src directory somewhere, example:&#x20;

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (75).png" alt="" width="375"><figcaption><p>The Keycloak logo has been replaced by the Keycloakify logo</p></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (76).png" alt="" width="297"><figcaption><p>Before</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (77).png" alt="" width="375"><figcaption><p>Commenting out deviceActivity</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (78).png" alt="" width="315"><figcaption><p>After, the Device Activity section has disapeared</p></figcaption></figure>

### Adding custom CSS

The official way of customizing the look of the Account UI is to overload the PaternFly CSS variables:&#x20;

{% embed url="https://www.patternfly.org/components/button/html/#css-variables" %}

But we are aware that this isn't enough for most of you. &#x20;

Beyond that you can of course import your custom CSS and use the PaternFly utility classes as target but be aware that theses classes can be changed in the future.

\
Example loading some custom CSS:

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>



[^1]: React, in developement mode, when using `<StrictMode/>`, renders the components twice, this is not a bug.
