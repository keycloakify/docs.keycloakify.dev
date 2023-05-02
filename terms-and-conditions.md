# ✒ Terms and conditions

{% embed url="https://user-images.githubusercontent.com/6702424/164942769-0d5420e3-c62f-4187-977e-316a113fb037.mp4" %}

Many organizations have a requirement that when a new user logs in for the first time, they need to agree to the terms and conditions of the website.

First you need to enable the required action on the Keycloak server admin console:\


![](https://user-images.githubusercontent.com/6702424/114280501-dad2e600-9a39-11eb-9c39-a225572dd38a.png)

Then you load your own therms in Markdown format like this: &#x20;

<pre class="language-tsx" data-title="KcApp.tsx"><code class="lang-tsx">import { lazy, Suspense } from "react";
import Fallback, { type PageProps } from "keycloakify/login";
import type { KcContext } from "./kcContext";
import { useI18n } from "./i18n";
<strong>import { useDownloadTerms } from "keycloakify/login";
</strong><strong>import tos_en_url from "./assets/tos_en.md";
</strong><strong>import tos_fr_url from "./assets/tos_fr.md";
</strong>
const DefaultTemplate = lazy(() => import("keycloakify/login/Template"));

export default function App(props: { kcContext: KcContext; }) {

    const { kcContext } = props;

    const i18n = useI18n({ kcContext });
    
<strong>    useDownloadTerms({
</strong><strong>	kcContext,
</strong><strong>	"downloadTermMarkdown": async ({ currentLanguageTag }) => {
</strong><strong>
</strong><strong>            const tos_url = (() => {
</strong><strong>                switch (currentLanguageTag) {
</strong><strong>                    case "fr": return tos_fr_url;
</strong><strong>                    default: return tos_en_url;
</strong><strong>                }
</strong><strong>            })();
</strong><strong>
</strong><strong>
</strong><strong>            if ("__STORYBOOK_ADDONS" in window) {
</strong><strong>                // NOTE: In storybook, when you import a .md file you get the content of the file.
</strong><strong>                // In Create React App on the other hand you get an url to the file.
</strong><strong>                return tos_url;
</strong><strong>            }
</strong><strong>
</strong><strong>            const markdownString = await fetch(tos_url).then(response => response.text());
</strong><strong>
</strong><strong>            return markdownString;
</strong><strong>	}
</strong><strong>    });
</strong>
    if (i18n === null) {
        //NOTE: Locales not yet downloaded, we could as well display a loading progress but it's usually a matter of milliseconds.
        return null;
    }

    return (
        &#x3C;Suspense>
            {(() => {
                switch (kcContext.pageId) {
                    default: return &#x3C;Fallback {...{ kcContext, i18n }} Template={DefaultTemplate} doUseDefaultCss />;      
                }
            })()}
        &#x3C;/Suspense>
    );

}
</code></pre>

You can also eject the terms.ftl page if you're not happy with the default look: &#x20;

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/main/src/keycloak-theme/login/pages/Login.tsx" %}

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-terms-and-conditions" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
