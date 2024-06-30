# --project

This option is for Monorepos. More specifically, monorepo system that works with a single package.json at the root of the project.

You can run every subcommand of the `keycloakify` CLI tool from the root of your Keycloakify project using the `--project` (or `-p`) option. Example with the `build` command:

```bash
npx keycloakify build -p <path>
```

`<path>` would be typically something like `packages/keycloak-theme`

{% content-ref url="../keycloakify-in-my-app/as-a-subproject-of-your-monorepo/" %}
[as-a-subproject-of-your-monorepo](../keycloakify-in-my-app/as-a-subproject-of-your-monorepo/)
{% endcontent-ref %}
