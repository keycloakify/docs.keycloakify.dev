# projectBuildDirPath

In the Creact React App setup the, when you run yarn build, a build/ directory is generated.  \
If, in your setup it's an other directory you can use this option: &#x20;

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "projectBuildDirPath": "a/b/c"
    }
}
```
{% endcode %}

By default it's `"build"`.
