# 🔧 Accessing the Server Environement Variables

Environment variables defined on the Keycloak server can be transferred to the theme. This allows for a degree of theme customization without necessitating a rebuild. This approach is particularly useful if multiple parties are reusing your theme. As an example, you can distribute a single .jar file to multiple customers, enabling them to modify certain aspect of the login page by defining specific environment variables.

Concretely the ide is to be able, if you start your Keycloak server like this:

<pre class="language-bash"><code class="lang-bash">docker run \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
<strong>    --env MY_ENV_VARIABLE='Value of my env variable' \
</strong>    -p 8080:8080 \
    docker-keycloak-with-theme
</code></pre>

To be able to access MY\_ENV\_VARIABLE value in your theme. (This is assuming you're using docker but for for Helm for example see [this](importing-your-theme-in-keycloak.md#using-helm)).

...Under construction. The Keycloakify build option is  `environmentVariables`
