# 📖 Build options

## `package.json` options



You can read [here](https://github.com/InseeFrLab/keycloakify/blob/832434095eac722207c55062fd2b825d1f691722/src/bin/build-keycloak-theme/BuildOptions.ts#L7-L16) the package.json fields that are used by Keyclaokify.

### `extraThemeProperties`

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

### bundler

_Introduced in 6.11.4_

Configure if you want Keycloakify to build the final `.jar` for you or not.

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "bundler": "none"
    }
}
```
{% endcode %}

Possibles values are:

* `"keycloakify"` (default): Keycloakify will build the .jar file.
* `"none"`: Keycloakify will not create a .jar file.
* `"mvn"` (legacy): Keycloakify will use Maven to bundle the .jar file. This option is to use only if you experience problem with "keycloakify". It require mvn to be installed. If you have to resort to this option [please open an issue about it](https://github.com/InseeFrLab/keycloakify/issues/new) so we can see wha't wrong with our way of building the `.jar` file.

You can also convigure this value using an environement variable:

```bash
KEYCLOAKIFY_BUNDLER=none npx keycloakify
```

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

### keycloakVersionDefaultAssets

Default: 11.0.3

{% hint style="warning" %}
Only use this param if you know what you are doing. [See related issue](https://github.com/keycloakify/keycloakify/issues/276).
{% endhint %}

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "keycloakVersionDefaultAssets": "21.0.1"
    }
}
```
{% endcode %}

### version

Configure the version that will appear in the `pom.xml` file.

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

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

### extraThemeNames

_Introduced in 7.12_

This option let you pack multiple themes variant in a single `.jar` bundle. In vanilla Keycloak themes you have the ability to extend a base theme. There is now an idiomatic way of achieving the same result by using this option.

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "extraThemeNames": [ 
            "keycloakify-starter-variant-1", 
            "keycloakify-starter-variant-2"
        ]
    }
}
```
{% endcode %}

This will make the theme variant appear in the Keycloak admin select input:

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

The theme name will be available on the `kcContext`:

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

You'll be able to implement different behaviour based on which theme variant is active:

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### `areAppAndKeycloakServerSharingSameDomain` (deprecated)

This option is only considered when building with [`--external-assets`](build-options.md#external-assets).

Set to `true` it tels Keycloakify that you have configured your reverse proxy so that your app and your Keycloak server are under the same domain, example:

* _https://example.com/auth_: Keycloak.
* _https://example.com (or https://example.com/x/y/z)_: Your App

Example:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
    "keycloakify": {
<strong>        "areAppAndKeycloakServerSharingSameDomain": true
</strong>    }
}
</code></pre>

When enabled you don't need to specify a `homepage` field in the `package.json`

## CLI options

Options that can be passed to the `npx keycloakify` command.

### `--silent`

Prevent the build command from generating outputs.

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-build-options" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
