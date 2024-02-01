# ⬆ v8 -> v9

* If it's not already the case you must now use the [`copy-keycloak-resources-to-public` postinstall script](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/package.json#L11).
* The [`keycloakVersionDefaultAssets`](https://docs.keycloakify.dev/v/v8/build-options#keycloakversiondefaultassets) has been removed in favor of [`loginThemeResourcesFromKeycloakVersion`](../build-options.md#loginthemeresourcesfromkeycloakversion). &#x20;
* You now must have `nvm` (Maven, `brew install maven`) installed to build your Keycloakify theme. The `bundler` build option has been removed in favor of [doCreateJar](../build-options.md#docreatejar).  (Note that Maven is present by default on the GitHub Action runners). &#x20;
* The [`extraThemeNames`](https://docs.keycloakify.dev/v/v8/build-options#extrathemenames) build option has been removed, if you have theme variant, simply pass an array to the [themeName option](../build-options.md#themename).
* Keycloakify now generates two jars `*.jar` and `retrocompat-*.jar`. [You should update your CI](https://github.com/keycloakify/keycloakify-starter/commit/c9aad1406502ba08c654ade4bfa95bf3a6e93830).
  * If you have no Account theme: You can use the `retrocompat-*.jar`, it will work on any Keycloak version.
  *   If you have an Account theme:&#x20;

      * You should use `retrocompat-*.jar` on any Keycloak version prior to 23. And select `retrocompat_<your theme>` when selecting in the dropdown when selecting your account theme. &#x20;
      * You should use `*.jar` on Keycloak 23 and up.

      Be aware that Acount theme does not work in Keycloak 22 (and only in this version).

\
