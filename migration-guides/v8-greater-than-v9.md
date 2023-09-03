# ⬆ v8 -> v9

* If it's not already the case you must now use the [`copy-keycloak-resources-to-public` postinstall script](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/package.json#L11).
* The `keycloakVersionDefaultAssets` has been removed in favor of [`loginThemeResourcesFromKeycloakVersion`](../build-options.md#loginthemeresourcesfromkeycloakversion). &#x20;
* You now must have `nvm` (Maven) installed to build your Keycloakify theme. The `bundler` build option has been removed in favor of [doCreateJar](../build-options.md#docreatejar). &#x20;
* The `extraThemeNames` build option has been removed, if you have theme variant, simply pass an array to the [themeName option](../build-options.md#themename).
* If you implement an account theme. In Keycloak version prior to 22.1 you must select `<your theme name>_retrocompat` in the dropdown to select your theme. If you know your theme will only be used in recent version of Keycloak you can set the [`doBuildRetrocompatAccountTheme`](../build-options.md#dobuildretrocompataccounttheme) to `false`.\
