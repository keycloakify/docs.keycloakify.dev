# ⬆ v8 -> v9





* You must now use the [`copy-keycloak-resources-to-public` postinstall script](https://github.com/keycloakify/keycloakify-starter/blob/92b20fe74154ef8cf037f4b156eb3b2e5264a074/package.json#L11).
* The `keycloakVersionDefaultAssets` has been removed in favor of [`loginThemeResourcesFromKeycloakVersion`](../build-options.md#loginthemeresourcesfromkeycloakversion). &#x20;
* You now must have `nvm` (Maven) installed to build your Keycloakify theme. The `bundler` build option has been removed. \
