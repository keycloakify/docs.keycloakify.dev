# ⬆️ v8 -> v9

* If it's not already the case you must now use the [`copy-keycloak-resources-to-public` postinstall script](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/package.json#L11). (Only in WebPack projects, with Vite it's done automatically by the plugin)!.
* The [`keycloakVersionDefaultAssets`](https://docs.keycloakify.dev/v/v8/build-options#keycloakversiondefaultassets) has been removed in favor of [`loginThemeResourcesFromKeycloakVersion`](../../configuration-options/#loginthemeresourcesfromkeycloakversion).
* You now must have `mvn` (Maven, `brew install maven`) installed to build your Keycloakify theme. The `bundler` build option has been removed in favor of [doCreateJar](../../configuration-options/#docreatejar). (Note that Maven is present by default on the GitHub Action runners). If you are building your theme within Docker [here are the instructions](../../importing-your-theme-in-keycloak.md#using-docker).
* The [`extraThemeNames`](https://docs.keycloakify.dev/v/v8/build-options#extrathemenames) build option has been removed, if you have theme variant, simply pass an array to the [themeName option](../../configuration-options/#themename).
* Keycloakify now generates two jars `*.jar` and `retrocompat-*.jar`. [You should update your CI](https://github.com/keycloakify/keycloakify-starter/commit/c9aad1406502ba08c654ade4bfa95bf3a6e93830).
  * If you have no Account theme: You can use the `retrocompat-*.jar`, it will work on any Keycloak version.
  *   If you have an Account theme:

      * You should use `retrocompat-*.jar` on any Keycloak version prior to 23. [And select `<your theme>_retrocompat` when selecting in the dropdown when selecting your account theme](https://github.com/keycloakify/keycloakify/assets/6702424/bc0c988a-9fc1-4c45-a37a-fcf98b7096af).
      * You should use `*.jar` on Keycloak 23 and up.

      Be aware that Account theme does not work in Keycloak 22 (and only in this version).
* If you where loading your theme into your Keycloak by copy pasting the build\_keycloak/src directly into your Keycloak you can still do that with Keycloak prior to 22 or if you don't have an account theme but if you have an account theme and you are on Keycloak 23 you must follow one of the [official instruction for loading the .jar into Keycloak](../../importing-your-theme-in-keycloak.md).

\\
