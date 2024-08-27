# In a Keycloak Docker Container

{% hint style="info" %}
TLDR:

```bash
npx keycloakify start-keycloak
```
{% endhint %}

Testing your theme in Storybook is nice, but at some point you'll want to test your theme in a real Keycloak before shipping it in production!

First you want to install and launch [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or just Docker) on your computer, if you haven't done it already.

You'll also need Maven to build the .jar locally. Try running `mvn --version` to see if you have it already. If you don't install it with:

{% tabs %}
{% tab title="MacOS" %}
Using [Homebrew](https://formulae.brew.sh/formula/maven):

```bash
brew install maven
```
{% endtab %}

{% tab title="Ubuntu/Debian" %}
```bash
sudo apt-get install maven
```
{% endtab %}

{% tab title="Windows" %}
On Windows you can use the [Chocolatery](https://chocolatey.org/) package manager:

```bash
choco install openjdk
choco install maven
```

Or [install it manually](https://chocolatey.org/).
{% endtab %}
{% endtabs %}

\
You are ready! In your Keycloakify just run:

```bash
npx keycloakify start-keycloak
```

You'll be invited to chose the Keycloak version you want to spin up:

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Keycloakify will preconfigure a realm and client for your theme so you don't necessarily need to go in the the Keycloak admin console you can simply navigate to **https://my-theme.keycloakify.dev** it will redirect to your local Keycloak login pages!

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
If you makes changes in your theme while the Keycloak container is running **your theme will be automatically recompiled** and updated in Keycloak. After a few seconds you'll just have to refresh the page you see them live.
{% endhint %}

Clicking on the **https://my-theme.keycloakify.dev** link will redirect you to the login page of your theme:

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

With the developer tool of your brower you'll be able to explore the kcContext of the page. You can use it to creates new stories of your pages in specific configuration.

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Loggin in with the test user (testuser/password123) will redirect you to a page where you'll be able to inspect the decoded id token JWT beside other things.

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

## Configuration options

There are many options available to you to configure the Keycloak testing container.

{% tabs %}
{% tab title="Vite" %}
<pre class="language-typescript" data-title="vite.config.ts"><code class="lang-typescript">import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
 plugins: [
  react(),
  keycloakify({
   accountThemeImplementation: "none",
<strong>   startKeycloakOptions: {
</strong><strong>    // All the options are optional
</strong><strong>
</strong><strong>    /*
</strong><strong>    By default Keycloakify uses the official Keycloak Docker Image (quay.io/keycloak/keycloak)
</strong><strong>    and lets you select the tag when you run the command by asking you to 
</strong><strong>    select a Keycloak version.
</strong><strong>    Using this option you can use an alternative Docker Image like the one 
</strong><strong>    of Phase2 (https://quay.io/repository/phasetwo/phasetwo-keycloak) and a 
</strong><strong>    specific tag to use.
</strong><strong>    This option can also be used to pin a specific version of the official 
</strong><strong>    Keycloak Docker Image.
</strong><strong>    Example: `dockerImage: "quay.io/keycloak/keycloak:25.0.2"`
</strong><strong>    */
</strong><strong>    dockerImage: "quay.io/phasetwo/phasetwo-keycloak:25.0.2.1721752809",
</strong><strong>
</strong><strong>    /*
</strong><strong>    This option allows you to pass extra docker arguments to the 
</strong><strong>    `docker run` command.
</strong><strong>    */
</strong><strong>    dockerExtraArgs: [
</strong><strong>     "-e", "KC_HTTP_RELATIVE_PATH=/auth"
</strong><strong>    ],
</strong><strong>
</strong><strong>    /*
</strong><strong>    This option allows you to start Keycloak with extra arguments.
</strong><strong>    */
</strong><strong>    keycloakExtraArgs: [
</strong><strong>     "--spi-email-template-provider=freemarker-plus-mustache",
</strong><strong>     "--spi-email-template-freemarker-plus-mustache-enabled=true",
</strong><strong>     "--spi-theme-cache-themes=false"
</strong><strong>    ],
</strong><strong>
</strong><strong>    /*
</strong><strong>    This option allow you to load custom Keycloak extensions in the 
</strong><strong>    Keycloak instance running in the Docker container.
</strong><strong>    In this example we load two extensions:
</strong><strong>    - https://github.com/InseeFr/Keycloak-FranceConnect
</strong><strong>    - https://github.com/micedre/keycloak-mail-whitelisting
</strong><strong>    */
</strong><strong>    extensionJars: [
</strong><strong>     "https://github.com/InseeFr/Keycloak-FranceConnect/releases/download/6.2.0/keycloak-franceconnect-6.2.0.jar",
</strong><strong>     "./keycloak-resources/keycloak-mail-whitelisting-2.0.jar"
</strong><strong>    ],
</strong><strong>    /*
</strong><strong>    By default, the Keycloak instance is loaded with a pre-configured 
</strong><strong>    realm so you do not have to create a realm, a client, a user, etc.  
</strong><strong>    However you might want to edit this base realm configuration and 
</strong><strong>    persist the changes that you made.  
</strong><strong>    I explain it in this video: https://www.youtube.com/watch?v=lMOLrdqilqE&#x26;t=991s
</strong><strong>    */
</strong><strong>    realmJsonFilePath: "./keycloak-resources/myrealm-realm.json",
</strong><strong>    /*
</strong><strong>     * By default the Keycloak instance will run on port 8080.
</strong><strong>     * This option allows you to change it to another port.
</strong><strong>     */
</strong><strong>    port: 8081
</strong><strong>   }
</strong><strong>  })
</strong><strong> ]
</strong><strong>});
</strong></code></pre>
{% endtab %}

{% tab title="Webpack" %}
{% code title="package.json" %}
```json
{
  "keycloakify": {
    "startKeycloakOptions": {
      "dockerImage": "quay.io/phasetwo/phasetwo-keycloak:25.0.2.1721752809",
      "dockerExtraArgs": [
        "-e",
        "KC_HTTP_RELATIVE_PATH=/auth"
      ],
      "keycloakExtraArgs": [
        "--spi-email-template-provider=freemarker-plus-mustache",
        "--spi-email-template-freemarker-plus-mustache-enabled=true",
        "--spi-theme-cache-themes=false"
      ],
      "extensionJars": [
        "https://github.com/InseeFr/Keycloak-FranceConnect/releases/download/6.2.0/keycloak-franceconnect-6.2.0.jar",
        "./keycloak-resources/keycloak-mail-whitelisting-2.0.jar"
      ],
      "realmJsonFilePath": "./keycloak-resources/myrealm-realm.json",
      "port": 8081
    }
  }
}
```
{% endcode %}

See the Vite tab for explaination of the different options.
{% endtab %}
{% endtabs %}

Configured as the example above, the docker run command will be the following one:

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>
