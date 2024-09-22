# XDG\_CACHE\_HOME

If this environnement variable is defined this cache directory will be used instead of the default `node_modules/.cache/keycloakify` example:

```bash
export XDG_CACHE_HOME=/home/runner/.cache/yarn
npx keycloakify build
# /home/runner/.cache/yarn/keycloakify will contain various resources
```

This option is mainly useful if you need to be able to build your theme offline, in a context with network restriction polices.

The Keycloakify caches the default Keycloak theme resources to avoid having to download them over and over.
