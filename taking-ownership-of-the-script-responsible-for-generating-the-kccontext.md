# 🤓 Taking ownership of the script responsible for generating the kcContext

{% hint style="warning" %}
Use this approach only if you know what you are doing. In most sincronstances you shouldn't need it.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=WdSPrpFObhg" %}

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
