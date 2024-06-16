# 📖 Build Options

### --project

This option is for Monorepos. You can run every subcommand of the `keycloakify` CLI tool from the root of your Keycloakify project with, example with the `build` command:

```bash
npx keycloakify build -p <path>
```

`<path>` would be typically something like `packages/keycloak-theme`

{% content-ref url="../keycloakify-in-my-app/as-a-subproject-of-your-monorepo.md" %}
[as-a-subproject-of-your-monorepo.md](../keycloakify-in-my-app/as-a-subproject-of-your-monorepo.md)
{% endcontent-ref %}

### postBuild

{% hint style="info" %}
Only avaiable in Vite projects
{% endhint %}

The postBuild hook is called just before Keycloakify bundles the themes resources into the jar. &#x20;

This gives you the ability to implement some custom transformation.&#x20;

Let's say, for example, we have a big `material-icons` in our `public` directory and thoses icons are used in the main app but not in the Keycloak theme.  We can use the postBuild hook to make sure that thoses icons are not bundled in the generated jar files.

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import * as fs from "fs/promises";

export default defineConfig({
    plugins: [react(), keycloakify({
<strong>        postBuild: async (buildContext) => {
</strong><strong>        
</strong><strong>            await fs.rm(
</strong><strong>                pathJoin(
</strong><strong>                    "theme",
</strong><strong>                    "onyxia",
</strong><strong>                    "login",
</strong><strong>                    "resources",
</strong><strong>                    "build",
</strong><strong>                    "material-icons"
</strong><strong>                ),
</strong><strong>                { "recursive": true }
</strong><strong>            );
</strong><strong>            
</strong><strong>        }
</strong>        
    })]
});
</code></pre>

When this function is invoked the current working directory (process.cwd()) is the root of the directory of the files about to be archived.

You can get an idea of how is structured the files inside the jar by extracting it manually:

```bash
mkdir dist_keycloak/extracted
cd dist_keycloak/extracted
jar -xf ../keycloak-theme-for-kc-25-and-above.jar
```

### groupId

_Introduced in 6.11_

Configure the `groupId` that will appear in the `pom.xml` file.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      groupId: "dev.keycloakify.demo-app-advanced.keycloak"
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
    "keycloakify": {
        "groupId": "dev.keycloakify.demo-app-advanced.keycloak"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

By default it's the package.json homepage field at reverse with .keycloak at the end.

You can overwrite this using an environment variable:

```bash
KEYCLOAKIFY_GROUP_ID="com.your-company.your-project.keycloak" npx keycloakify
```

### artifactId

_Introduced in 6.11_

Configure the `artifactId` that will appear in the `pom.xml` file.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      artifactId: "keycloakify-advanced-starter-keycloak-theme"
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
    "keycloakify": {
        "artifactId": "keycloakify-advanced-starter-keycloak-theme"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

By default it's `<themeName>-keycloak-theme` See, [`keycloak.themeName`](./#keyclokify.themename) option.

You can overwrite this using an environment variable:

```bash
KEYCLOAKIFY_ARTIFACT_ID="my-cool-theme" npx keycloakify
```

{% hint style="info" %}
The `artifactId` also affects [the name of the `.jar` file](https://github.com/InseeFrLab/keycloakify/blob/9f72024c61b1b36d71a42b242c05d7ac793e049b/src/bin/keycloakify/generateJavaStackFiles.ts#L85).
{% endhint %}

### PUBLIC\_DIR\_PATH

> Only relevant in webpack project, in Vite, it's read from your vite.config.ts file!

Default: `~/public`

Example: `npx PUBLIC_DIR_PATH=./web/public npx keycloakify copy-keycloak-resources-to-public`
