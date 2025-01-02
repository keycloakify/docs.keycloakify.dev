# 🤓 Taking ownership of the kcContext

This documentation explore how to finely controlls what is and isnt included in the `window.kcContext` object.

{% hint style="info" %}
If you simply want to **remove** some specific values from the kcContext you can use the [kcContextExclusionFtl](configuration-options/kccontextexclusionsftl.md) option. &#x20;
{% endhint %}

Some values, like for example the realm attributes (kcContext.realm.attributes) are explicitely excluded from the KcContext.

In the following video we explore how to include them back. &#x20;

{% embed url="https://www.youtube.com/watch?v=WdSPrpFObhg" %}

Note that in the video we includes **all** the realm attributes. We might want to expose only a specific set of values. For this we could do: &#x20;

{% code title="node_modules/keycloakify/src/bin/keycloakify/generateFtl/kcContextDeclarationTemplate.ftl" %}
```diff
-    ) || (
-        key == "attributes" &&
-        areSamePath(path, ["realm"])
-    ) || (
+    ) || (
+        areSamePath(path, ["realm", "attributes"]) &&
+        !["myFirstAttribute", "mySecondAttribute"]?seq_contains(key)
+    ) || (
```
{% endcode %}

We could also chose to include only the realm attributes with a specific prefix, for example `theme_`:

{% code title="node_modules/keycloakify/src/bin/keycloakify/generateFtl/kcContextDeclarationTemplate.ftl" %}
```diff
-    ) || (
-        key == "attributes" &&
-        areSamePath(path, ["realm"])
-    ) || (
+    ) || (
+        areSamePath(path, ["realm", "attributes"]) &&
+        !key?starts_with("theme_")
+    ) || (
```
{% endcode %}

## Setting up patch-package

As explained in the video: &#x20;

Add [patch-package](https://www.npmjs.com/package/patch-package) add dev dependency

```bash
yarn add --dev patch-package
```

Edit the FreeMarker template that generates the KcContext in:&#x20;

**node\_modules/keycloakify/src/bin/keycloakify/generateFtl/kcContextDeclarationTemplate.ftl**

You can then create a diff for your changes by running: &#x20;

```bash
npx patch-package keycloakify
```

Then add a postinstall script to your package.json:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "name": "keycloakify-starter",
    "scripts": {
<strong>        "postinstall": "patch-package",
</strong>        "dev": "vite",
</code></pre>

Commit the **patch/** directory that have been created by patch-package.
