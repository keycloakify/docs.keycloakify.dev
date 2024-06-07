# 📖 Build options

### --project or -p CLI option

Introduced in Keycloakify 9.4

This option is for Monorepos. You can run Keycloakify from the root of your Keycloakify project with:

`npx keycloakify build -p <path>`

`<path>` would be typically something like `packages/keycloak-theme`

See:

{% content-ref url="keycloakify-in-my-app/monorepo.md" %}
[monorepo.md](keycloakify-in-my-app/monorepo.md)
{% endcontent-ref %}

### postBuild hook

_Ony available in Vite, introduced in v9.5_

The postBuild hook is an optional parameter of the Keycloakify Vite plugin that enables you to apply some transformation to your Keycloak theme after it's been built but before the .jar is created.

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
      themeName: "keycloakify-starter",
      extraThemeProperties: [
        "foo=bar"
      ],
<strong>      // In this example, after running `yarn build-keycloak-theme`
</strong><strong>      // there will be a `keycloak_dist/foo.txt` file.  
</strong><strong>      postBuild: async keycloakifyBuildOptions => {
</strong>
<strong>        const fs = await import("fs/promises");
</strong><strong>        const path = await import("path");
</strong>
<strong>        await fs.writeFile(
</strong><strong>          path.join(keycloakifyBuildOptions.keycloakifyBuildDirPath, "foo.txt"),
</strong><strong>          Buffer.from(
</strong><strong>            [
</strong><strong>            "Created by the postBuild hook of the keycloakify vite plugin", 
</strong><strong>            "",
</strong><strong>            "Resolved keycloakifyBuildOptions:",
</strong><strong>            "",
</strong><strong>            JSON.stringify(keycloakifyBuildOptions, null, 2),
</strong><strong>            ""
</strong><strong>            ].join("\n"),
</strong><strong>            "utf8"
</strong><strong>          )
</strong><strong>        );
</strong>
<strong>      }
</strong>    })
  ],
</code></pre>

### extraThemeProperties

This let you add properties to the `build_keycloak/src/main/resources/theme/<your app>/[login|account]/theme.properties` file.

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      extraThemeProperties: [ 
</strong><strong>        "locales=en,ko",
</strong><strong>        "MY_ENV_VARIABLE=${env.MY_ENV_VARIABLE:default}"
</strong><strong>      ]
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
<strong>        "extraThemeProperties": [ 
</strong><strong>            "locales=en,ko",
</strong><strong>            "MY_ENV_VARIABLE=${env.MY_ENV_VARIABLE:default}"
</strong><strong>        ]
</strong>    }
}
</code></pre>
{% endtab %}
{% endtabs %}

It is mainly useful to get access to the Keycloak server environment variables in your theme. See:

{% content-ref url="environment-variables.md" %}
[environment-variables.md](environment-variables.md)
{% endcontent-ref %}

### groupId

_Introduced in 6.11_

Configure the `groupId` that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

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

By default it's `<themeName>-keycloak-theme` See, [`keycloak.themeName`](build-options.md#keyclokify.themename) option.

You can overwrite this using an environment variable:

```bash
KEYCLOAKIFY_ARTIFACT_ID="my-cool-theme" npx keycloakify
```

{% hint style="info" %}
The `artifactId` also affects [the name of the `.jar` file](https://github.com/InseeFrLab/keycloakify/blob/9f72024c61b1b36d71a42b242c05d7ac793e049b/src/bin/keycloakify/generateJavaStackFiles.ts#L85).
{% endhint %}

### version

Configure the version that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

By default the version that is used is the one in the package.json of your project

{% code title="package.json" %}
```json
{
    "version": "1.3.4"
}
```
{% endcode %}

But you can overwrite this value using an environment variable (_Introduced in 6.11)_:

```bash
KEYCLOAKIFY_THEME_VERSION="4.5.6" npx keycloakify build
```

### themeName

_Introduced in 7.5.0_

By default it's `package.json["name"]`

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      themeName: "my-custom-name"
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
        "themeName": "my-custom-name"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

By providing an array this also enables you to implement theme variant. See:\


{% content-ref url="theme-variants.md" %}
[theme-variants.md](theme-variants.md)
{% endcontent-ref %}

### XDG\_CACHE\_HOME

If this environnement variable is defined this cache directory will be used instead of `node_modules/.cache` example:

```bash
export XDG_CACHE_HOME=/home/runner/.cache/yarn
npx keycloakify build
# /home/runner/.cache/yarn/keycloakify will contain various resources
```

### PUBLIC\_DIR\_PATH

> Only relevant in webpack project, in Vite, it's read from your vite.config.ts file!

Default: `~/public`

Example: `npx PUBLIC_DIR_PATH=./web/public npx keycloakify copy-keycloak-resources-to-public`

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-build-options" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud-IAM consulting services to simplify your experience.
{% endembed %}
