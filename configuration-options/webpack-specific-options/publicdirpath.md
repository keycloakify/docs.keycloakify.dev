# publicDirPath

To enable to test your theme locally, in storybook or with yarn start Keycloakify copies the default theme resources, primarily constituted of PatternFly, the CSS framework used for the default theme. &#x20;

This option allows you to customize what's the public directory in your case. By default it's `public/` but in angular for example it's `src/assets/`.

{% code title="package.json" %}

```json
{
  "keycloakify": {
    "publicDirPath": "./public"
  }
}
```

{% endcode %}

Default: `~/public`

You can also use the `PUBLIC_DIR_PATH` environnement variable. Example:

```bash
npx PUBLIC_DIR_PATH=./src/assets keycloakify copy-keycloak-resources-to-public
```
