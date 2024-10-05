# 🖇️ Styling a Custom Page Not Included In Base Keycloak

Sometimes certain extensions will add new functionality that requires an additional page not originally shipped with Keycloak. Keycloakify out-of-the-box will only provide customization to base pages, so if a new page is introduced by an extension, there is a good chance the page will not be styled correctly.

To account for these cases, Keycloakify supports the ability to add custom pages and configure them such that style preservation is maintained.

For our example on how to customize this, we will be using Phase Two's otp-form.ftl page. Phase Two provides email OTP codes for logging in and as a result has a special page if OTP codes are enabled in the authorization flow. &#x20;

{% hint style="success" %}
You can load the extention that you are using in Keycloak container that is started when running `npx keycloakify start-keycloak`. Use [the `extensionJars option`](testing-your-theme/in-a-keycloak-docker-container.md).
{% endhint %}

{% embed url="https://github.com/p2-inc/keycloak-magic-link/blob/main/src/main/resources/theme-resources/templates/otp-form.ftl" %}
You can find the original .ftl file on Phase Two's github
{% endembed %}

The first thing we will do is create the page under the pages directory, our file name in this case will be `OtpForm.tsx` and paste in some starter code including the template.

{% code title="src/login/pages/OtpForm.tsx" %}
```tsx
import { getKcClsx } from "keycloakify/login/lib/kcClsx";
import type { PageProps } from "keycloakify/login/pages/PageProps";
import type { KcContext } from "../KcContext";
import type { I18n } from "../i18n";

export default function OtpForm(props: PageProps<Extract<KcContext, { pageId: "otp-form.ftl" }>, I18n>) {
    const { kcContext, i18n, doUseDefaultCss, Template, classes } = props;

    const { kcClsx } = getKcClsx({
        doUseDefaultCss,
        classes
    });

    const { msg, msgStr } = i18n;

    const { url } = kcContext;


    return (
        <Template
            kcContext={kcContext}
            i18n={i18n}
            doUseDefaultCss={doUseDefaultCss}
            classes={classes}
            displayInfo={false}
            headerNode={
                // Header code goes here
            }
        >
            // Page code goes here
        </Template>
    );
}
```
{% endcode %}

Note the `pageId` variable specified `otp-form.ftl`, that should match the exact name of the page file you are trying to implement. Additionally, we will also need to modify the `kcContext` values to account for certain custom variables, but we will get to that later. For now the last new file we need to add would be the story file for this page:

{% code title="src/login/pages/OtpForm.stories.tsx" %}
```tsx
import type { Meta, StoryObj } from "@storybook/react";
import { createKcPageStory } from "../KcPageStory";

const { KcPageStory } = createKcPageStory({ pageId: "otp-form.ftl" });

const meta = {
    title: "login/otp-form.ftl",
    component: KcPageStory
} satisfies Meta<typeof KcPageStory>;

export default meta;

type Story = StoryObj<typeof meta>;

export const Default: Story = {
    render: () => <KcPageStory />
};
```
{% endcode %}

Next the easiest thing is to just paste the default code for the custom page right into the template and begin modifying it for Keycloakify. In our case, here is the code for that page at the time of writing:

<details>

<summary>otp-form.ftl</summary>

```xml
<#import "template.ftl" as layout>
<@layout.registrationLayout displayInfo=true; section>
    <#if section = "title">
        ${msg("doLogIn")}

    <#elseif section = "header">
      <div id="kc-username" class="${properties.kcFormGroupClass!}">
        <label id="kc-attempted-username">${auth.attemptedUsername}</label>
        <a id="reset-login" href="${url.loginRestartFlowUrl}" aria-label="${msg("restartLoginTooltip")}">
          <div class="kc-login-tooltip">
            <i class="${properties.kcResetFlowIcon!}"></i>
            <span class="kc-tooltip-text">${msg("restartLoginTooltip")}</span>
          </div>
        </a>
      </div>

    <#elseif section = "form">
      <p>Enter access code</p>
      <form id="kc-otp-login-form" class="${properties.kcFormClass!}" action="${url.loginAction}" method="post">
        <div class="${properties.kcFormGroupClass!}">
          <div class="${properties.kcLabelWrapperClass!}">
            <label for="otp" class="${properties.kcLabelClass!}">${msg("loginOtpOneTime")}</label>
          </div>

          <div class="${properties.kcInputWrapperClass!}">
            <input id="otp" name="otp" autocomplete="off" type="text" class="${properties.kcInputClass!}" autofocus aria-invalid="<#if messagesPerField.existsError('totp')>true</#if>"/>
            <#if messagesPerField.existsError('totp')>
              <span id="input-error-otp-code" class="${properties.kcInputErrorMessageClass!}" aria-live="polite">${kcSanitize(messagesPerField.get('totp'))?no_esc}</span>
            </#if>
          </div>
        </div>

        <div class="${properties.kcFormGroupClass!}">
          <div id="kc-form-options" class="${properties.kcFormOptionsClass!}">
            <div class="${properties.kcFormOptionsWrapperClass!}">
            </div>
          </div>

          <div id="kc-form-buttons" class="${properties.kcFormButtonsClass!}">
            <input class="${properties.kcButtonClass!} ${properties.kcButtonPrimaryClass!} ${properties.kcButtonLargeClass!}" name="submit" id="kc-submit" type="submit" value="${msg("doSubmit")}" />
            <input class="${properties.kcButtonClass!} ${properties.kcButtonPrimaryClass!} ${properties.kcButtonLargeClass!}" name="resend" id="kc-resend" type="submit" value="${msg("doResend")}" />
          </div>
        </div>
      </form>
    </#if>
</@layout.registrationLayout>
```

</details>

Breaking down this code:

1. The freemarker, dynamic variables/messages, and classnames will need to be converted to React.
2. The content in the header section will go in the `headerNode` prop of `<Template>` and the form section will be the child of the `<Template>` element.
3. `@layout.registrationLayout` has the prop `displayInfo=true` which means we need to set that prop in the `<Template>` element.
4. The `auth` and `messagesPerField` variables and their attributes which need to be provided in kcContext.

1, 2, and 3 require converting code to JSX. The converted code for the page can be found at the bottom. Here are some tips:

* Any classname provided as a variable will use `kcClsx` to resolve, so `${properties.kcFormClass!}` would turn into `{kcClsx("kcFormGroupClass")}`
* When dealing with message values, `msg` may return full blown HTML so it can be used as a child element and `msgStr` will return straight text.
  * Example 1, `aria-label="${msg("restartLoginTooltip")}"` would turn into `aria-label={msgStr("restartLoginTooltip")}`.
  *   Example 2, `msg` variables they can inject HTML as a variable, when this happens we need to dangerously set inner html. Specifcally with a piece of code like this:

      ```html
      <#if messagesPerField.existsError('totp')>
        <span id="input-error-otp-code" class="${properties.kcInputErrorMessageClass!}" aria-live="polite">
          ${kcSanitize(messagesPerField.get('totp'))?no_esc}
        </span>
      </#if>
      ```

      would turn into this:

      ```jsx
      {
        messagesPerField.existsError("totp") && (
          <span
            id="input-error-otp-code"
            className={kcClsx("kcInputErrorMessageClass")}
            aria-live="polite"
            dangerouslySetInnerHTML={{ __html: messagesPerField.get("totp") }}
          />
        );
      }
      ```

      Unfortunately, a lot of it is up to you to decide with the extension you might be using, but there may be some trial and error.

4 on the other hand requires changing some code in other files.

{% code title="src/login/KcContext.ts" %}
```tsx
/* eslint-disable @typescript-eslint/ban-types */
import type { ExtendKcContext } from "keycloakify/login";
import type { KcEnvName, ThemeName } from "../kc.gen";

export type KcContextExtension = {
    themeName: ThemeName;
    properties: Record<KcEnvName, string> & {};
};

// added for otp form page, required for the types
export type KcContextExtensionPerPage = {
    "otp-form.ftl": {
        auth: {
            attemptedUsername: string;
        };
        url: {
            loginRestartFlowUrl: string;
            loginAction: string;
        };
    };
};

export type KcContext = ExtendKcContext<KcContextExtension, KcContextExtensionPerPage>;
```
{% endcode %}

As seen above, kcContext is where we can add the type definitions for the props passed into the page. In the freemarker we also see `msg("doResend")` value which is not in the base keycloak i18 library. We would also need add this for mocking purposes.

<pre class="language-typescript" data-title="src/login/i18n.ts"><code class="lang-typescript">import { i18nBuilder } from "keycloakify/login";
import type { ThemeName } from "../kc.gen";

/** @see: https://docs.keycloakify.dev/i18n */
const { useI18n, ofTypeI18n } = i18nBuilder
    .withThemeName&#x3C;ThemeName>()
    .withExtraLanguages({ /* ... */ })
    .withCustomTranslations({
<strong>        en: {
</strong><strong>            doResend: "Resend"
</strong><strong>        },
</strong><strong>        fr: {
</strong><strong>            doResend: "Renvoyer"
</strong><strong>        }
</strong>    })
    .build();

type I18n = typeof ofTypeI18n;

export { useI18n, type I18n };
</code></pre>

The last two things we need to do now would be adding the story to the `KcPageStory.tsx`

{% code title="src/login/KcPageStory.tsx" %}
```typescript
const kcContextExtensionPerPage: KcContextExtensionPerPage = {
    "otp-form.ftl": {
        auth: {
            attemptedUsername: "user@user.com"
        },
        url: {
            loginRestartFlowUrl: "#",
            loginAction: "#"
        }
    }
};
```
{% endcode %}

and adding the page to the `KcPage.tsx`

{% code title="src/login/KcPage.tsx" %}
```tsx
case "otp-form.ftl":
    return (
        <OtpForm
            {...{ kcContext, i18n, classes }}
            Template={Template}
            doUseDefaultCss={true}
        />
    );
```
{% endcode %}

After all that you should be done! You can view the new component in storybook and check everything looks right and then the next time you bundle and build it, it should be deployed.

<details>

<summary>Completed code for OtpForm.tsx:</summary>

```JSX
import { getKcClsx } from "keycloakify/login/lib/kcClsx";
import type { PageProps } from "keycloakify/login/pages/PageProps";
import type { KcContext } from "../KcContext";
import type { I18n } from "../i18n";

export default function OtpForm(props: PageProps<Extract<KcContext, { pageId: "otp-form.ftl" }>, I18n>) {
    const { kcContext, i18n, doUseDefaultCss, Template, classes } = props;

    const { kcClsx } = getKcClsx({
        doUseDefaultCss,
        classes
    });

    const { msg, msgStr } = i18n;

    const { auth, url, messagesPerField } = kcContext;

    return (
        <Template
            kcContext={kcContext}
            i18n={i18n}
            doUseDefaultCss={doUseDefaultCss}
            classes={classes}
            displayInfo={false}
            headerNode={
                <div id="kc-username" className={kcClsx("kcFormGroupClass")} style={{ fontSize: "16px" }}>
                    <label id="kc-attempted-username">{auth.attemptedUsername}</label>
                    <a id="reset-login" href={url.loginRestartFlowUrl} aria-label={msgStr("restartLoginTooltip")}>
                        <div className="kc-login-tooltip">
                            <i className={kcClsx("kcResetFlowIcon")}></i>
                            <span className="kc-tooltip-text">{msg("restartLoginTooltip")}</span>
                        </div>
                    </a>
                </div>
            }
        >
            <p>Enter access code</p>
            <form id="kc-otp-login-form" className={kcClsx("kcFormClass")} action={url.loginAction} method="post">
                <div className={kcClsx("kcFormGroupClass")}>
                    <div className={kcClsx("kcLabelWrapperClass")}>
                        <label htmlFor="otp" className={kcClsx("kcLabelClass")}>
                            {msg("loginOtpOneTime")}
                        </label>
                    </div>

                    <div className={kcClsx("kcInputWrapperClass")}>
                        <input
                            id="otp"
                            name="otp"
                            autoComplete="off"
                            type="text"
                            className={kcClsx("kcInputClass")}
                            autoFocus
                            aria-invalid={messagesPerField.existsError("totp") ? "true" : undefined}
                        />
                        {messagesPerField.existsError("totp") && (
                            <span
                                id="input-error-otp-code"
                                className={kcClsx("kcInputErrorMessageClass")}
                                aria-live="polite"
                                dangerouslySetInnerHTML={{ __html: messagesPerField.get("totp") }}
                            />
                        )}
                    </div>
                </div>

                <div className={kcClsx("kcFormGroupClass")}>
                    <div id="kc-form-options" className={kcClsx("kcFormOptionsClass")}>
                        <div className={kcClsx("kcFormOptionsWrapperClass")} />
                    </div>

                    <div id="kc-form-buttons" className={kcClsx("kcFormButtonsClass")}>
                        <input
                            className={kcClsx("kcButtonClass", "kcButtonPrimaryClass", "kcButtonLargeClass")}
                            name="submit"
                            id="kc-submit"
                            type="submit"
                            value={msgStr("doSubmit")}
                        />
                        <input
                            className={kcClsx("kcButtonClass", "kcButtonPrimaryClass", "kcButtonLargeClass")}
                            name="resend"
                            id="kc-resend"
                            type="submit"
                            value={msgStr("doResend")}
                        />
                    </div>
                </div>
            </form>
        </Template>
    );
}
```

</details>
