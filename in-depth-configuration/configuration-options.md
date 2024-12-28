# Configuration Options

On this page different configuration options are listed you can use with Keycloakify. &#x20;

## --project

This option is for Monorepos. More specifically, monorepo system that works with a single package.json at the root of the project.

You can run every subcommand of the `keycloakify` CLI tool from the root of your Keycloakify project using the `--project` (or `-p`) option. Example with the `build` command:

```bash
npx keycloakify build -p <path>
```

`<path>` would be typically something like `packages/keycloak-theme`

## keycloakVersionTargets

{% content-ref url="targeting-specific-keycloak-versions.md" %}
[targeting-specific-keycloak-versions.md](targeting-specific-keycloak-versions.md)
{% endcontent-ref %}

## environmentVariables

{% content-ref url="environment-variables.md" %}
[environment-variables.md](environment-variables.md)
{% endcontent-ref %}

## themeName

This is the name that will appear in the select input of the Keycloak Admin UI that let's you select the theme.

<figure><img src="../.gitbook/assets/image (65).png" alt="" width="375"><figcaption><p>Here the theme name is "my-react-app"</p></figcaption></figure>

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

The theme name is also in `kcContext.themeName`

Providing an array enables you to implement theme variant. See:

{% content-ref url="theme-variants.md" %}
[theme-variants.md](theme-variants.md)
{% endcontent-ref %}

## themeVersion

{% content-ref url="../basics/testing-your-theme/in-a-keycloak-docker-container.md" %}
[in-a-keycloak-docker-container.md](../basics/testing-your-theme/in-a-keycloak-docker-container.md)
{% endcontent-ref %}

## postBuild

{% hint style="info" %}
Only available in Vite projects, not in Webpack
{% endhint %}

The postBuild hook is called just before Keycloakify bundles the themes resources into the jar.

This gives you the ability to implement some custom transformation.

Let's say, for example, we have a big `material-icons` in our `public` directory and those icons are used in the main app but not in the Keycloak theme. We can use the postBuild hook to make sure that those icons are not bundled in the generated jar files.

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

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption><p>Overview of the content of the jar extracted</p></figcaption></figure>

### Debugging

Note that the script is executed in a different thread. console.log() won't work.  \
If you want to debug you can write your logs into a file.&#x20;

{% code title="vite.config.ts" %}
```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
            accountThemeImplementation: "Single-Page",
            postBuild: async (buildContext) => {

                const fs = await import("fs");
                const path = await import("path");

                const logFilePath = path.join(buildContext.projectDirPath, "postBuildLog.txt");

                fs.rmSync(logFilePath, { force: true });

                const log= (msg: string) => {
                    fs.appendFileSync(
                        logFilePath,
                        Buffer.from(msg + "\n", "utf8")
                    );
                };

                // This will be logged into postBuildLog.txt at the root of your project.
                log("Hello World");

            }
        })
    ]
});

```
{% endcode %}

## XDG\_CACHE\_HOME

If this environnement variable is defined this cache directory will be used instead of the default `node_modules/.cache/keycloakify` example:

```bash
export XDG_CACHE_HOME=/home/runner/.cache/yarn
npx keycloakify build
# /home/runner/.cache/yarn/keycloakify will contain various resources
```

This option is mainly useful if you need to be able to build your theme offline, in a context with network restriction polices.

The Keycloakify caches the default Keycloak theme resources to avoid having to download them over and over.

## kcContextExclusionsFtl

[Keycloakify shifts page generation from the backend to the client](https://github.com/keycloakify/keycloakify/discussions/346#discussioncomment-5889791). To achieve this, Keycloakify creates a global `kcContext` object, which holds the necessary information for generating HTML pages.

This object contains no sensitive data—only the information that the Keycloak team considers essential for rendering the various login pages. Additionally, if you have custom plugins, such as [keycloak-email-whitelisting](https://github.com/micedre/keycloak-mail-whitelisting), they may introduce additional values into this object.

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption><p>A typical kcContext for the register.ftl page</p></figcaption></figure>

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

## More advanced modification of the kcContext

If you feel limited by this option you can take ownership of the FreeMarker template that generates the kcContext.  \
\
Do do this using the wonderfull [patch-package](https://www.npmjs.com/package/patch-package).

```bash
yarn add --dev patch-package
```

And edit the FreeMarker template that generates the KcContext in:&#x20;

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

## keycloakifyBuildDirPath

This option enables you to configure in which directory the .jar files should be created.&#x20;

{% tabs %}
{% tab title="Vite" %}
By default it's **./dist\_keycloak**

<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { keycloakify } from "keycloakify/vite-plugin";

export default defineConfig({
  plugins: [
    react(), 
    keycloakify({
<strong>      keycloakifyBuildDirPath: "./keycloak-theme-dist"
</strong>    })
  ],
})
</code></pre>
{% endtab %}

{% tab title="Webpack" %}
By default it's **./build\_keycloak**

{% code title="package.json" %}
```json
"keycloakify": {
    "keycloakifyBuildDirPath": "./keycloak-theme-jars"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## groupId

Configure the `groupId` that will appear in the `pom.xml` file.

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

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

## artifactId

{% hint style="info" %}
NOTE: For changing the name of the jar file that is generated by Keycloakify see this option instead: [keycloakVersionTargets](targeting-specific-keycloak-versions.md).
{% endhint %}

Configure the `artifactId` that will appear in the `pom.xml` file.

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

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

By default it's `<themeName>-keycloak-theme` See, [`keycloak.themeName`](configuration-options.md#keyclokify.themename) option.

You can overwrite this using an environment variable:

```bash
KEYCLOAKIFY_ARTIFACT_ID="my-cool-theme" npx keycloakify build
```

## Webpack specific options

In this sub folder are listed the few build options that are only relevant in Webpack project.

Be aware, theses are not preferences, they have to reflect your webpack configuration!

### projectBuildDirPath

In the Create React App setup, when you run yarn build, a build/ directory is generated.\
If, in your setup it's an other directory you can use this option:a

{% code title="package.json" %}
```json
"keycloakify": {
  "projectBuildDirPath": "a/b/c"
}
```
{% endcode %}

By default it's `build`.

### staticDirPathInProjectBuildDirPath

In the Creact React App setup, when you run yarn build, a build/ directory is generated.  \
I this directory there's a static directory/.  \
If, in your setup it's an other directory you can use this option: &#x20;

{% code title="package.json" %}
```json
"keycloakify": {
    "staticDirPathInProjectBuildDirPath": "a/b/c"
}
```
{% endcode %}

By default it's `static`.

### publicDirPath

To enable to test your theme locally, in storybook or with yarn start Keycloakify copies the default theme resources, primarily constituted of PatternFly, the CSS framework used for the default theme.

This option allows you to customize what's the public directory in your case. By default it's `public/` but in angular for example it's `src/assets/`.

{% code title="package.json" %}
```json
"keycloakify": {
  "publicDirPath": "./public"
}
```
{% endcode %}

By default it is `~/public`

You can also use the `PUBLIC_DIR_PATH` environnement variable. Example:

```bash
npx PUBLIC_DIR_PATH=./src/assets keycloakify copy-keycloak-resources-to-public
```
