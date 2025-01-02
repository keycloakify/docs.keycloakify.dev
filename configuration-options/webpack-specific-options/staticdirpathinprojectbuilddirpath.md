# staticDirPathInProjectBuildDirPath

In the Creact React App setup the, when you run yarn build, a build/ directory is generated.  \
I this directory there's a static directory/.  \
If, in your setup it's an other directory you can use this option: &#x20;

{% code title="package.json" %}
```json
{
    "keycloakify": {
        "staticDirPathInProjectBuildDirPath": "a/b/c"
    }
}
```
{% endcode %}

By default it's `"static"`.
