# ⬆ v7 -> v8

* Remove the `public/keycloak-resources` directory.
* The [`--external-assets` build option has been removed](https://docs.keycloakify.dev/v/v7/build-options#external-assets-deprecated) it was a performance optimization that is no longer relevant now that we have lazy loading.
* `kcContext.usernameEditDisabled` is now `kcContext.usernameHidden`, the type was lying, it has been updated to reflect what's actually on the `kcContext` at runtime. If you want to see in detail what should be updated [see issue](https://github.com/keycloakify/keycloakify/pull/399), or you can search and replace `usernameEditDisabled` -> `usernameHidden` it'll do the trick.
* The `usePrepareTemplate` prototype has been changed, you can search and replace:

`src/keycloak-theme/login/Template.tsx`

```ts
url,
"stylesCommon": [
    "node_modules/patternfly/dist/css/patternfly.min.css",
    "node_modules/patternfly/dist/css/patternfly-additions.min.css",
    "lib/zocial/zocial.css"
],
"styles": ["css/login.css"],
```

by

```ts
"styles": [
    `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly.min.css`,
    `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly-additions.min.css`,
    `${url.resourcesCommonPath}/lib/zocial/zocial.css`,
    `${url.resourcesPath}/css/login.css`
],
```

and

`src/keycloak-theme/account/Template.css`

```ts
url,
"stylesCommon": ["node_modules/patternfly/dist/css/patternfly.min.css", "node_modules/patternfly/dist/css/patternfly-additions.min.css"],
"styles": ["css/account.css"],
```

by

```ts
"styles": [
    `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly.min.css`,
    `${url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly-additions.min.css`,
    `${url.resourcesPath}/css/account.css`
],
```
