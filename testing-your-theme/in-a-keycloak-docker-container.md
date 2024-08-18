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
If you makes changes in your theme while the Keycloak container is running your theme will be automatically recompiled and updated in Keycloak. After a few seconds you'll just have to refresh the page you see them live.
{% endhint %}

You'll be able to authenticate with the test user:

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

And see the content of your Open ID Connect Access Token JWT:

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Here you can chose to go back to the login page or you have a link to the account pages:

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Now what's interesting is you can see the real kcContext in the dev tools. You can use it as a reference to create new stories!

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## Options

{% tabs %}
{% tab title="Vite" %}
{% code title="vite.config.ts" %}
```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
 plugins: [
  react(),
  keycloakify({
   accountThemeImplementation: "none",
   startKeycloakOptions: {
    // All the options are optional

    /*
    By default Keycloakify uses the official Keycloak Docker Image (quay.io/keycloak/keycloak)
    and lets you select the tag when you run the command by asking you to 
    select a Keycloak version.
    Using this option you can use an alternative Docker Image like the one 
    of Phase2 (https://quay.io/repository/phasetwo/phasetwo-keycloak) and a 
    specific tag to use.
    This option can also be used to pin a specific version of the official 
    Keycloak Docker Image.
    Example: `dockerImage: "quay.io/keycloak/keycloak:25.0.2"`
    */
    dockerImage: "quay.io/phasetwo/phasetwo-keycloak:25.0.2.1721752809",

    /*
    This option allows you to pass extra docker arguments to the 
    `docker run` command.
    */
    dockerExtraArgs: [
     "-e", "KC_HTTP_RELATIVE_PATH=/auth"
    ],

    /*
    This option allows you to start Keycloak with extra arguments.
    */
    keycloakExtraArgs: [
     "--spi-email-template-provider=freemarker-plus-mustache",
     "--spi-email-template-freemarker-plus-mustache-enabled=true",
     "--spi-theme-cache-themes=false"
    ],

    /*
    This option allow you to load custom Keycloak extensions in the 
    Keycloak instance running in the Docker container.
    In this example we load two extensions:
    - https://github.com/InseeFr/Keycloak-FranceConnect
    - https://github.com/micedre/keycloak-mail-whitelisting
    */
    extensionJars: [
     "https://github.com/InseeFr/Keycloak-FranceConnect/releases/download/6.2.0/keycloak-franceconnect-6.2.0.jar",
     "./keycloak-resources/keycloak-mail-whitelisting-2.0.jar"
    ],
    /*
    By default, the Keycloak instance is loaded with a pre-configured 
    realm so you do not have to create a realm, a client, a user, etc.  
    However you might want to edit this base realm configuration and 
    persist the changes that you made.  
    For this, after you've made changes in the Keycloak admin console, 
    you can navigate to:
    Realm Settings -> Actions -> Partial Export, enable "export clients" and click "Export"
    This will download a JSON file that you can use to restore your 
    changes the next time you run `npx keycloakify start-keycloak`
    */
    realmJsonFilePath: "./keycloak-resources/myrealm-realm.json",
    /*
     * By default the Keycloak instance will run on port 8080.
     * This option allows you to change it to another port.
     */
    port: 8081
   }
  })
 ]
});

```
{% endcode %}
{% endtab %}

{% tab title="Webpack" %}

{% endtab %}
{% endtabs %}



## Exporting your realm configuration

If you connect to the Keycloak Admin UI (**http://localhost:8080**) and make some changes to the Ream configuration you can then export theses change into a .json file using the partial export feature of Keycloak:

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption><p>When exporting, enable "export clients"</p></figcaption></figure>

Later on, you can then start another container again with the same configuration using:

```bash
npx keycloakify start-keycloak --import realm-export.json
```
