# startKeycloakOptions

You can configure the Keycloak testing container using options in your build tool configuration.

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
    I explain it in this video: https://www.youtube.com/watch?v=lMOLrdqilqE&t=991s
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

See the **Vite** tab for explanations of the options.
{% endtab %}
{% endtabs %}

Configured as above, the resulting Docker command will resemble the following:

<figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

dd
