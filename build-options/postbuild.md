# postBuild

{% hint style="info" %}
Only avaiable in Vite projects, not in Webpack
{% endhint %}

The postBuild hook is called just before Keycloakify bundles the themes resources into the jar. &#x20;

This gives you the ability to implement some custom transformation.&#x20;

Let's say, for example, we have a big `material-icons` in our `public` directory and thoses icons are used in the main app but not in the Keycloak theme.  We can use the postBuild hook to make sure that thoses icons are not bundled in the generated jar files.

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import * as fs from "fs/promises";
import * as path from "path";

export default defineConfig({
    plugins: [
        react(),
        keycloakify({
<strong>            postBuild: async (buildContext) => {
</strong><strong>                await fs.rm(
</strong><strong>                    path.join(
</strong><strong>                        "theme",
</strong><strong>                        buildContext.themeNames[0], // keycloakify-starter
</strong><strong>                        "login", // Note: We assume we only have an login theme, if we had an account theme we would have to remove it there as well.
</strong><strong>                        "resources",
</strong><strong>                        "dist", // Your Vite dist/ or Webpack build/ is here.
</strong><strong>                        "material-icons"
</strong><strong>                    ),
</strong><strong>                    { recursive: true }
</strong><strong>                );
</strong><strong>            }
</strong>        })
    ]
});
</code></pre>

When this function is invoked the current working directory (process.cwd()) is the root of the directory of the files about to be archived.

You can get an idea of how is structured the files inside the jar by extracting it manually:

```bash
mkdir dist_keycloak/extracted
cd dist_keycloak/extracted
jar -xf ../keycloak-theme-for-kc-25-and-above.jar
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Overview of the content of the jar extracted</p></figcaption></figure>
