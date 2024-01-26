# 📖 Build options

### extraThemeProperties

By default the `theme.properties` files located in `build_keycloak/src/main/resources/theme/<your app>/login/theme.properties` only contains:

```
parent=keycloak
```

If, for some reason, you need to add extra properties like for example `env=dev` you can do it by editing your `package.json` this way:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
<strong>        "extraThemeProperties": [ 
</strong><strong>            "env=dev",
</strong><strong>            "locales=en,ko",
</strong><strong>            "foo=bar",
</strong><strong>            "myValue=${env.MY_ENV_VARIABLE:default}"
</strong><strong>        ]
</strong>    }
}
</code></pre>

You can then access this property in the `kcContext` (`kcContext.properties.foo === "bar"`) even if you won't have type safety.

If you want to have your custom properties listed on the kcContext (at the type level) you can augment the KcContext type definition. [More info](https://github.com/keycloakify/keycloakify/issues/229#issuecomment-1635883568).

You can also use it to access Keycloak environment variables in your theme. [More info](https://github.com/keycloakify/keycloakify/issues/288).\
\
You can find [here](https://github.com/codegouvfr/sill-web/commit/01a58e06a007fa06499214f87d9d207981c3bbf7) a practical example of environment variables.

### doCreateJar

default: true

_Introduced in 9.0_

Tell wether or not you want Keycloakify to bundle your theme within a .jar file.

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "doCreateJar": false
    }
}
```
{% endcode %}

### groupId

_Introduced in 6.11_

Configure the `groupId` that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "groupId": "dev.keycloakify.demo-app-advanced.keycloak"
    }
}
```
{% endcode %}

By default it's the package.json hompage field at reverse with .keycloak at the end.

You can overwrite this using an environement variable:

```bash
KEYCLOAKIFY_GROUP_ID="com.your-company.your-project.keycloak" npx keycloakify
```

### artifactId

_Introduced in 6.11_

Configure the `artifactId` that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "artifactId": "keycloakify-advanced-starter-keycloak-theme"
    }
}
```
{% endcode %}

By default it's `<themeName>-keycloak-theme` See, [`keycloak.themeName`](build-options.md#keyclokify.themename) option.

You can overwrite this using an environement variable:

```bash
KEYCLOAKIFY_ARTIFACT_ID="my-cool-theme" npx keycloakify
```

{% hint style="info" %}
The `artifactId` also affects [the name of the `.jar` file](https://github.com/InseeFrLab/keycloakify/blob/9f72024c61b1b36d71a42b242c05d7ac793e049b/src/bin/keycloakify/generateJavaStackFiles.ts#L85).
{% endhint %}

### loginThemeResourcesFromKeycloakVersion

Default: 11.0.3

This replaces `keycloakVersionDefaultAssets`.

The default login `Template.tsx` imports CSS resources that are copied from the Keycloak version specified by this parameter.\
This is not something you should worry about too much. These imports are mostly there so that the pages that Keycloakify provides by default match the ones of the default theme. You should, however, strive to use your own assets; after all, this is the point of creating a theme.   \
\
Example where Keycloak resources are imported in the login theme: &#x20;

* &#x20;[In Template.tsx, css imports](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/src/keycloak-theme/login/Template.tsx#L37-L40)
* [In LoginOtp, js import](https://github.com/keycloakify/keycloakify/blob/402c6fc64a26268b6f2f7222e4f11ff07de452f8/src/login/pages/LoginOtp.tsx#L26)

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "loginThemeResourcesFromKeycloakVersion": "21.0.1"
    }
}
```
{% endcode %}

Note that for account theme we do not enable to specify the version, the assets used are fixed to Keycloak 21.1.2. &#x20;

### version

Configure the version that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

By default the version that is used is the one in the package.json of your project

{% code title="package.json" %}
```json
{
    "version": "1.3.4"
}
```
{% endcode %}

But you can overwrite this value using an environnement variable (_Introduced in 6.11)_:

```bash
KEYCLOAKIFY_THEME_VERSION="4.5.6" npx keycloakify
```

{% hint style="info" %}
The version also affects [the name of the `.jar` file](https://github.com/InseeFrLab/keycloakify/blob/9f72024c61b1b36d71a42b242c05d7ac793e049b/src/bin/keycloakify/generateJavaStackFiles.ts#L85).
{% endhint %}

### **customUserAttributes**

_Deprecated._

_Introduced in 7.4.0 removed in 7.13.0_

Keycloakify now analyzes your code and see what field name are actually used.\
Just make sure your fieldNames aren't generated at runtime. Eg:

```tsx
// OK ✅
messagesPerField.exists("foo-bar")

// Not OK 🛑
const bar= "bar";
messagesPerField.exists(`foo-${bar}`);
```

[See issue for more context](https://github.com/keycloakify/keycloakify/issues/40).

### themeName

_Introduced in 7.5.0_

This is the name of the theme in the Keycloak admin select:

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

By default it's `package.json["name"]`

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "themeName": "my-custom-name"
    }
}
```
{% endcode %}

You can also provide an array if you want to Keycloakify to create multiple theme variant:&#x20;

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "themeName": [ "keycloakify-starter", "keycloakify-starter-variant-1" ]
    }
}
```
{% endcode %}

This option deprecates `extraThemeNames`and let you pack multiple themes variant in a single `.jar` bundle. In vanilla Keycloak themes you have the ability to extend a base theme. There is now an idiomatic way of achieving the same result by using this option.

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

The theme name will be available on the `kcContext`:

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

You'll be able to implement different behaviour based on which theme variant is the current one:

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

To load different global css file based on the theme name you can implement this strategy: &#x20;

```typescript
// src/keycloak-theme/login/useGlobalStylesheet.ts
import { useMemo } from "react";

export function useGlobalStylesheet(themeName: string){
    useMemo(() => {
        switch(themeName){
            case "keycloakify-starter":
                // @ts-expect-error
                import("./keycloakify-starter.css");
                break;
            case "keycloakify-starter-variant-1":
                // @ts-expect-error
                import("./keycloakify-starter-variant-1.css");
                break;
        }
    }, [themeName]);
}
```

```tsx
// src/keycloak-theme/login/KcApp.tsx

import { useGlobalStylesheet } from "./useGlobalStylesheet";
// ...

export default function KcApp(props: { kcContext: KcContext; }) {
  const { kcContext }= props;
  // ...
  
  useGlogalStylesheet(kcContext.themeName);

  // ...
```

### silent

Options that can be passed to the `npx keycloakify` command. With `npx keycloakify --silent` no output is printed to the console.

### XDG\_CACHE\_HOME

Keycloakify needs to download resources from the Keycloak project to build your theme. To prevent these resources from being downloaded repeatedly, Keycloakify caches them by default in `node_modules/.cache`. However, you can specify a different location by setting the `XDG_CACHE_HOME` environment variable.\
Example: `XDG_CACHE_HOME=/home/runner/.cache/yarn npx keycloakify`

This is particularly useful [in your CI workflow](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/.github/workflows/ci.yaml#L19-L21) to ensure that the cache persists across runs (see the documentation for the bahmutov/npm-install GitHub Action).

### PUBLIC\_DIR\_PATH

Default: `~/public`

Example: `npx PUBLIC_DIR_PATH=./web/public npx copy-keycloak-resources-to-public`

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-build-options" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
