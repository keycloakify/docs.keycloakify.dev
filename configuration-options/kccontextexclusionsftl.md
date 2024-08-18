# kcContextExclusionsFtl

[Keycloakify shifts page generation from the backend to the client](https://github.com/keycloakify/keycloakify/discussions/346#discussioncomment-5889791). To achieve this, Keycloakify creates a global `kcContext` object, which holds the necessary information for generating HTML pages.

This object contains no sensitive data—only the information that the Keycloak team considers essential for rendering the various login pages. Additionally, if you have custom plugins, such as [keycloak-email-whitelisting](https://github.com/micedre/keycloak-mail-whitelisting), they may introduce additional values into this object.

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption><p>A typical kcContext for the register.ftl page</p></figcaption></figure>

If you'd like to prevent some values of the FreeMarker context from being forwarded to the client you can do it with the `kcContextExclusionsFtl` option. &#x20;

Let's say in this example that we would like to exclude:

* `kcContext.keycloakifyVersion`
* `kcContext.realm.actionTokenGeneratedByUserLifespanMinutes` in the register.ftl page
* `kcContext.realm.idpVerifyAccountLinkActionTokenLifespanMinutes` in the register.ftl page

This is how you would do it:

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
            // ...
<strong>            kcContextExclusionsFtl: `
</strong><strong>                &#x3C;#if (
</strong><strong>                    key == "keycloakifyVersion" &#x26;&#x26;
</strong><strong>                    areSamePath(path, []) 
</strong><strong>                )>
</strong><strong>                    &#x3C;#continue>
</strong><strong>                &#x3C;/#if>
</strong><strong>                &#x3C;#if (
</strong><strong>                    xKeycloakify.pageId == "register.ftl" &#x26;&#x26;
</strong><strong>                    [
</strong><strong>                        "actionTokenGeneratedByUserLifespanMinutes", 
</strong><strong>                        "idpVerifyAccountLinkActionTokenLifespanMinutes"
</strong><strong>                    ]?seq_contains(key) &#x26;&#x26;
</strong><strong>                    areSamePath(path, ["realm"]
</strong><strong>                )>
</strong><strong>                    &#x3C;#continue>
</strong><strong>                &#x3C;/#if>
</strong>            `
        })
    ]
});
</code></pre>

{% hint style="info" %}
You can also provide a path to a .ftl file instead of inlining the ftl code in your vite.config.ts file.
{% endhint %}
{% endtab %}

{% tab title="Webpack" %}
{% code title="kcContextExclusions.ftl" %}
```ftl
<#if (
    key == "keycloakifyVersion" &&
    areSamePath(path, []) 
)>
    <#continue>
</#if>
<#if (
    xKeycloakify.pageId == "register.ftl" &&
    [
        "actionTokenGeneratedByUserLifespanMinutes", 
        "idpVerifyAccountLinkActionTokenLifespanMinutes"
    ]?seq_contains(key) &&
    areSamePath(path, ["realm"]
)>
    <#continue>
</#if>
```
{% endcode %}

<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
        // ...
<strong>        "kcContextExclusionsFtl": "./kcContextExclusions.ftl"
</strong>    }
}
</code></pre>
{% endtab %}
{% endtabs %}

The code that you provide will be injected [here](https://github.com/keycloakify/keycloakify/blob/0879ddba7c3e12ea5ffd0cbefd97fa9b8cf63f7e/src/bin/keycloakify/generateFtl/kcContextDeclarationTemplate.ftl#L249). &#x20;

For more detailed example you can refer to this section of the code that defines the the default exclusions:

{% embed url="https://github.com/keycloakify/keycloakify/blob/0879ddba7c3e12ea5ffd0cbefd97fa9b8cf63f7e/src/bin/keycloakify/generateFtl/kcContextDeclarationTemplate.ftl#L125-L227" %}
