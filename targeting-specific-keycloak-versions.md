# 🎯 Targetting Specific Keycloak Versions

{% embed url="https://youtu.be/WiiG42jn5T0" %}



By default, Keycloakify generates diffrent jar files, each one meant to be used with a given Keycloak version range.

<figure><img src=".gitbook/assets/image (164).png" alt="" width="375"><figcaption></figcaption></figure>

However you might want to customize this behavior. If you know ahead of time what Keycloak you theme will using you can build only for this version using the `keycloakVersionTargets` build option.

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react(), keycloakify({
        // ...
<strong>        keycloakVersionTargets: {
</strong><strong>            // It depends of your configuration
</strong><strong>            // Watch the video to learn more
</strong><strong>        }
</strong>    })]
});
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
        // ...
<strong>        "keycloakVersionTargets": {
</strong><strong>           // It depends of your configuration, if you are implementing
</strong><strong>           // an Multi-Page account theme or not and of the version
</strong><strong>           // of keycloakify you are using.  
</strong><strong>           // Since TypeScript can't help you here the best option 
</strong><strong>           // to know what ranges are available is to clone the vite
</strong><strong>           // starter, pin your Keycloakify specific version and 
</strong><strong>           // set the account implementation that you have in your project.
</strong><strong>           // Watch the video to learn more.
</strong><strong>           // The vite starter: https://github.com/keycloakify/keycloakify-starter
</strong>    }
}
</code></pre>
{% endtab %}
{% endtabs %}
