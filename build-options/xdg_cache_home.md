# XDG\_CACHE\_HOME

If this environnement variable is defined this cache directory will be used instead of `node_modules/.cache` example:

```bash
export XDG_CACHE_HOME=/home/runner/.cache/yarn
npx keycloakify build
# /home/runner/.cache/yarn/keycloakify will contain various resources
```

This option is mainly usefull if you need to be able to build your theme offline, in a context with strong network restriction polices. &#x20;
