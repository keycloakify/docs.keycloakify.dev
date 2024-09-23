# ⬆️ v10->v11

Don't worry, this is not like v9 to v10, this update is very easy.  \


See release note:

{% embed url="https://github.com/keycloakify/keycloakify/releases" %}

{% hint style="warning" %}
Pay close attention. The syntax hylighting of gitbook for diffs is buggy. Don't look only at the colors. Look at the `+` and `-` symbols at the start of each line.
{% endhint %}

## Changes to the i18n system

<pre class="language-diff" data-title="src/login/i18n.ts"><code class="lang-diff">-import { createUseI18n } from "keycloakify/login";
+import { i18nBuilder } from "keycloakify/login";
+import type { ThemeName } from "../kc.gen";


<strong>-export const { useI18n, ofTypeI18n } = createUseI18n({
</strong>+const { useI18n, ofTypeI18n } = i18nBuilder
+    .withThemeName&#x3C;ThemeName>()
+    .withCustomTranslations({
        en: {
          backToLogin: "⏪ Back to &#x3C;strong>Login page&#x3C;/strong>",
          myCustomKey: "My custom message",
        },
        fr: {
          backToLogin: "⏪ Retour à la &#x3C;strong>page de Login&#x3C;/strong>",
          myCustomKey: "Mon message personalisé",
        },
     })
+    .build()
      
-export type I18n = typeof ofTypeI18n;
+type I18n = typeof ofTypeI18n;

+export { useI18n, type I18n };
</code></pre>

If you have ejected the Template.tsx the language selector code has changed a little: &#x20;

{% code title="src/login/Template.tsx" %}
```diff
-   const { realm, locale, auth, url, message, isAppInitiatedAction } = kcContext;
+   const { realm, auth, url, message, isAppInitiatedAction } = kcContext;
-   const { msg, msgStr, currentLanguageTag } = i18n;
+   const { msg, msgStr, currentLanguage, enabledLanguages } = i18n;

    // ...

    return (
        <div className={kcClsx("kcLoginClass")}>
            <div id="kc-header" className={kcClsx("kcHeaderClass")}>
                <div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
                    {msg("loginTitleHtml", realm.displayNameHtml)}
                </div>
            </div>

            <div className={kcClsx("kcFormCardClass")}>
                <header className={kcClsx("kcFormHeaderClass")}>
-                   {realm.internationalizationEnabled && (assert(locale !== undefined), locale.supported.length > 1) && (
+                   {enabledLanguages.length > 1 && (
                        <div className={kcClsx("kcLocaleMainClass")} id="kc-locale">
                            <div id="kc-locale-wrapper" className={kcClsx("kcLocaleWrapperClass")}>
                                <div id="kc-locale-dropdown" className={clsx("menu-button-links", kcClsx("kcLocaleDropDownClass"))}>
                                    <button
                                        tabIndex={1}
                                        id="kc-current-locale-link"
                                        aria-label={msgStr("languages")}
                                        aria-haspopup="true"
                                        aria-expanded="false"
                                        aria-controls="language-switch1"
                                    >
-                                        {labelBySupportedLanguageTag[currentLanguageTag]}
+                                        {currentLanguage.label}
                                    </button>
                                    <ul
                                        role="menu"
                                        tabIndex={-1}
                                        aria-labelledby="kc-current-locale-link"
                                        aria-activedescendant=""
                                        id="language-switch1"
                                        className={kcClsx("kcLocaleListClass")}
                                    >
-                                       {locale.supported.map(({ languageTag }, i) => (
-                                           <li key={languageTag} className={kcClsx("kcLocaleListItemClass")} role="none">
-                                               <a
-                                                   role="menuitem"
-                                                   id={`language-${i + 1}`}
-                                                   className={kcClsx("kcLocaleItemClass")}
-                                                   href={getChangeLocaleUrl(languageTag)}
-                                               >
-                                                   {labelBySupportedLanguageTag[languageTag]}
+                                       {enabledLanguages.map(({ languageTag, label, href }, i) => (
+                                           <li key={languageTag} className={kcClsx("kcLocaleListItemClass")} role="none">
+                                               <a role="menuitem" id={`language-${i + 1}`} className={kcClsx("kcLocaleItemClass")} href={href}>
+                                                   {label}
                                                </a>
                                            </li>
                                        ))}
                                    </ul>
                                </div>
                            </div>
                        </div>
                    )}
```
{% endcode %}

## Some components logic have been abstracted away

This only applies if you have ejected the mentioned components.

{% code title="src/login/Template.tsx" %}
```diff
import { useEffect } from "react";
import { assert } from "keycloakify/tools/assert";
import { clsx } from "keycloakify/tools/clsx";
import type { TemplateProps } from "keycloakify/login/TemplateProps";
import { getKcClsx } from "keycloakify/login/lib/kcClsx";
-import { useInsertScriptTags } from "keycloakify/tools/useInsertScriptTags";
-import { useInsertLinkTags } from "keycloakify/tools/useInsertLinkTags";
+import { useInitialize } from "keycloakify/login/Template.useInitialize";
import { useSetClassName } from "keycloakify/tools/useSetClassName";
import type { I18n } from "./i18n";
import type { KcContext } from "./KcContext";

export default function Template(props: TemplateProps<KcContext, I18n>) {
    const {
        displayInfo = false,
        displayMessage = true,
        displayRequiredFields = false,
        headerNode,
        socialProvidersNode = null,
        infoNode = null,
        documentTitle,
        bodyClassName,
        kcContext,
        i18n,
        doUseDefaultCss,
        classes,
        children
    } = props;

    const { kcClsx } = getKcClsx({ doUseDefaultCss, classes });

    const { msg, msgStr, getChangeLocaleUrl, labelBySupportedLanguageTag, currentLanguageTag } = i18n;

    const { realm, locale, auth, url, message, isAppInitiatedAction, authenticationSession, scripts } = kcContext;

    useEffect(() => {
        document.title = documentTitle ?? msgStr("loginTitle", kcContext.realm.displayName);
    }, []);

    useSetClassName({
        qualifiedName: "html",
        className: kcClsx("kcHtmlClass")
    });

    useSetClassName({
        qualifiedName: "body",
        className: bodyClassName ?? kcClsx("kcBodyClass")
    });

-   useEffect(() => {
-       const { currentLanguageTag } = locale ?? {};
-       if (currentLanguageTag === undefined) {
-           return;
-       }
-       const html = document.querySelector("html");
-       assert(html !== null);
-       html.lang = currentLanguageTag;
-   }, []);

-   const { areAllStyleSheetsLoaded } = useInsertLinkTags({
-       componentOrHookName: "Template",
-       hrefs: !doUseDefaultCss
-           ? []
-           : [
-                 `${url.resourcesCommonPath}/node_modules/@patternfly/patternfly/patternfly.min.css`,
-                 `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly.min.css`,
-                 `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly-additions.min.css`,
-                 `${url.resourcesCommonPath}/lib/pficon/pficon.css`,
-                 `${url.resourcesPath}/css/login.css`
-             ]
-   });

-   const { insertScriptTags } = useInsertScriptTags({
-       componentOrHookName: "Template",
-       scriptTags: [
-           {
-               type: "module",
-               src: `${url.resourcesPath}/js/menu-button-links.js`
-           },
-           ...(authenticationSession === undefined
-               ? []
-               : [
-                     {
-                         type: "module",
-                         textContent: [
-                             `import { checkCookiesAndSetTimer } from "${url.resourcesPath}/js/authChecker.js";`,
-                             ``,
-                             `checkCookiesAndSetTimer(`,
-                             `  "${authenticationSession.authSessionId}",`,
-                             `  "${authenticationSession.tabId}",`,
-                             `  "${url.ssoLoginInOtherTabsUrl}"`,
-                             `);`
-                         ].join("\n")
-                     } as const
-                 ]),
-           ...scripts.map(
-               script =>
-                   ({
-                       type: "text/javascript",
-                       src: script
-                   }) as const
-           )
-       ]
-   });

-   useEffect(() => {
-       if (areAllStyleSheetsLoaded) {
-           insertScriptTags();
-       }
-   }, [areAllStyleSheetsLoaded]);

-   if (!areAllStyleSheetsLoaded) {
-       return null;
-   }

+   const { isReadyToRender } = useInitialize({ kcContext, doUseDefaultCss });

+   if (!isReadyToRender) {
+       return null;
+   }

    return (
        <div className={kcClsx("kcLoginClass")}>
```
{% endcode %}

Similar changes have been made to the following pages:

* [import { useScript } from "keycloakify/login/pages/LoginPasskeysConditionalAuthenticate.useScript";](https://github.com/keycloakify/keycloakify/blob/346fd7175faad17b36f66f2abcc968a3c9899f45/src/login/pages/LoginPasskeysConditionalAuthenticate.tsx#L5)
* [import { useScript } from "keycloakify/login/pages/LoginRecoveryAuthnCodeConfig.useScript";](https://github.com/keycloakify/keycloakify/blob/346fd7175faad17b36f66f2abcc968a3c9899f45/src/login/pages/LoginRecoveryAuthnCodeConfig.tsx#L3)
* [import { useScript } from "keycloakify/login/pages/WebauthnAuthenticate.useScript";](https://github.com/keycloakify/keycloakify/blob/346fd7175faad17b36f66f2abcc968a3c9899f45/src/login/pages/WebauthnAuthenticate.tsx#L4)
* [import { useScript } from "keycloakify/login/pages/WebauthnRegister.useScript";](https://github.com/keycloakify/keycloakify/blob/346fd7175faad17b36f66f2abcc968a3c9899f45/src/login/pages/WebauthnRegister.tsx#L2)

## kcSanitize

Keycloakify now implements a kcSanitize function analogous to the one used by the default Keycloak theme. &#x20;

You can search/replace in your codebase for any `dangerouslySetInnerHTML` occurence and wrap the html string into the `kcSanitize` method:

```diff
+import { kcSanitize } from "keycloakify/lib/kcSanitize";

// ...

            <span
                className="kc-feedback-text"
                dangerouslySetInnerHTML={{
-                   __html: message.summary
+                   __html: kcSanitize(message.summary)
                }}
            />
```

## keycloakify-resouces has been renamed keycloakify-dev-resources

{% hint style="warning" %}
This change is only required for Webpack users. Not Vite users. &#x20;
{% endhint %}

{% code title="package.json" %}
```diff
 {
     "scripts": {
         "prestart": "keycloakify update-kc-gen && keycloakify copy-keycloak-resources-to-public",
         "start": "react-scripts start",
         "prestorybook": "npm run prestart",
         "storybook": "storybook dev -p 6006",
         "prebuild": "keycloakify update-kc-gen",
         "build": "react-scripts build",
-        "postbuild": "rimraf build/keycloakify-resources",
+        "postbuild": "rimraf build/keycloakify-dev-resources",
         "build-keycloak-theme": "npm run build && keycloakify build",
         "format": "prettier . --write"
         // ...
     },
```
{% endcode %}

