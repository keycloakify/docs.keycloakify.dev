---
icon: list-check
---

# Deploying Your Theme

This section explains how to load and enable your theme in your production Keycloak instance.

{% hint style="warning" %}
**Warning:**\
If your goal is to test your theme in a Keycloak docker container for development purpose, this is NOT the correct section of the documentation.\
Refer to the [Testing Your Theme in a Keycloak Docker Container](testing-your-theme/inside-of-keycloak.md) section for detailed instructions.\
This section is intended for deploying your theme to a **production** Keycloak instance, which involves a completely different process.
{% endhint %}

## Building the JAR File

Keycloak uses an extension system where themes or other custom plugins are packaged as standardized JAR files.

The first step is to build your theme into a JAR file that can be loaded into Keycloak.

```bash
npm run build-keycloak-theme
```

This command will create a **/dist\_keycloak** directory containing the necessary JAR files.

<figure><img src=".gitbook/assets/image (197).png" alt="" width="375"><figcaption></figcaption></figure>

By default, Keycloakify generates multiple JAR files to support different Keycloak versions. Here’s how to choose the correct JAR file for your production environment:

• **Keycloak 11 to 21 and 26 and newer**: Use **keycloak-theme-for-kc-all-other-versions.jar**.

• **Keycloak 22 to 25**: Use **keycloak-theme-for-kc-22-to-25.jar**.

You can configure which JAR files are generated and how they are named. For details, refer to this guide:

{% content-ref url="features/compiler-options/keycloakversiontargets.md" %}
[keycloakversiontargets.md](features/compiler-options/keycloakversiontargets.md)
{% endcontent-ref %}

If you have an OPS team and your responsibility is limited to developing the theme, your job ends here. The JAR file is your deliverable. You can provide it to the person managing your Keycloak instance—they will know what to do with it.

If you are responsible for both development and deployment, keep reading to learn how to load and enable the theme in Keycloak.

## Loading the JAR File into Keycloak

Now that your theme is packaged as a JAR file, you can load it into your Keycloak server, just like any other Keycloak extension.

For official guidance, refer to the [Keycloak documentation on registering provider implementations](https://www.keycloak.org/docs/latest/server_development/#registering-provider-implementations).\
\
While the official documentation provides a general overview, you might wonder how to apply those instructions in practice. Below, you’ll find a few code snippets illustrating how to load your theme, depending on the method you use to deploy Keycloak in production.

{% hint style="warning" %}
Improtrant note:

**How to deploy Keycloak in production is beyond the scope of Keycloakify’s documentation**.

If you’re unfamiliar with deploying a Keycloak instance, we strongly recommend starting with [the official Keycloak deployment guides](https://www.keycloak.org/documentation).

Do **not** attempt to use these snippets directly without understanding how Keycloak deployment works.

Once you’re confident in deploying Keycloak, revisit this section to integrate your custom theme seamlessly.
{% endhint %}

{% tabs %}
{% tab title="Docker" %}
One of the most common ways to deploy Keycloak in production is by using the official Docker image.

If you are following this approach, you can use the `-v` option to mount your JAR file into the `/opt/keycloak` directory inside the container.

Here’s an example of how to run the Keycloak container with your custom theme:

<pre class="language-bash"><code class="lang-bash">docker run \
    # ...other options
<strong>    -v "./dist_keycloak/keycloak-theme-for-kc-all-other-versions.jar":/opt/keycloak/providers/keycloak-theme.jar \
</strong>    quay.io/keycloak/keycloak:26.0.7 \
    start
</code></pre>
{% endtab %}

{% tab title="Docker Compose" %}
This approach builds on the basic Docker setup, providing a more streamlined way to manage your Keycloak deployment with Docker Compose.\
Let’s assume you have the following directory structure:

```
./docker-compose.yaml
./themes/keycloak-theme-for-kc-all-other-versions.jar
```

{% code title="docker-compose.yaml" %}
```yaml
version: '3.7'

services:
  postgres:
    image: postgres:16.2
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    ports:
      - 5432:5432
    networks:
      - keycloak_network

  keycloak:

    image: quay.io/keycloak/keycloak:26.0.4
    command: start-dev

    environment:
      KC_HOSTNAME: ${KEYCLOAK_HOSTNAME}
      KC_HOSTNAME_PORT: 8080
      KC_HTTP_ENABLED: true
      KC_HEALTH_ENABLED: true
      KC_HOSTNAME_STRICT_HTTPS: false
      KC_HOSTNAME_STRICT: false

      KC_BOOTSTRAP_ADMIN_USERNAME: ${KEYCLOAK_ADMIN}
      KC_BOOTSTRAP_ADMIN_PASSWORD: ${KEYCLOAK_ADMIN_PASSWORD}
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres/${POSTGRES_DB}
      KC_DB_USERNAME: ${POSTGRES_USER}
      KC_DB_PASSWORD: ${POSTGRES_PASSWORD}
    ports:
      - 8080:8080
    volumes:
      - ./themes:/opt/keycloak/providers/
    restart: unless-stopped
    depends_on:
      - postgres
    networks:
      - keycloak_network

volumes:
  postgres_data:
    driver: local

networks:
  keycloak_network:
    driver: bridge
```
{% endcode %}
{% endtab %}

{% tab title="Helm" %}
If you use [Bitnami's Keycloak Helm chart](https://github.com/bitnami/charts/tree/main/bitnami/keycloak) you can leverage the initContainers parameter to load your theme.

{% code title="Chart.yaml" %}
```yaml
apiVersion: v2
name: keycloak
version: 1.0.0
dependencies:
  - name: keycloak
    version: 24.4.10 # Keycloak 26.1.2
    repository: oci://registry-1.docker.io/bitnamicharts
```
{% endcode %}

Here we only list the relevant values:

{% code title="values.yaml" %}
```yaml
# OPTIONAL: Here you can define environment variables that you can access in your theme, see: https://docs.keycloakify.dev/features/environment-variables
extraEnvVars:
  - name: MY_APP_PALLET
    value: "monokai"

initContainers:
  - name: realm-ext-provider
    image: curlimages/curl
    imagePullPolicy: IfNotPresent
    command:
      - sh
    args:
      - -c
      - |
        # Replace USER and PROJECT, use the correct version of the jar for the keycloak version you are deploying
        mkdir -p /emptydir/app-providers-dir
        curl -L -f -S -o /emptydir/app-providers-dir/keycloak-theme.jar https://github.com/USER/PROJECT/releases/download/VERSION/keycloak-theme-for-kc-all-other-versions.jar

    volumeMounts:
      - name: empty-dir
        mountPath: /emptydir
```
{% endcode %}

Read [this section of the starter project readme](https://github.com/keycloakify/keycloakify-starter?tab=readme-ov-file#github-actions) to learn how to get GitHub Action to publish your theme's JAR as assets of your GitHub release.
{% endtab %}

{% tab title="Bare metal" %}
What you need to know is that your keycloak-theme.jar should be placed in the provider directory of your Keycloak (e.g: `/opt/keycloak/providers)`\
After that you should run bin/kc.sh build (e.g: `sh /opt/keycloak/bin/kc.sh build`)

Then you can start your Keycloak server, your theme should be available in it.
{% endtab %}

{% tab title="Docker - Custom Image" %}
Another common approach is to build a custom Docker image of Keycloak that extends the official Keycloak image and includes your theme.

<pre class="language-bash"><code class="lang-bash">cd ~/github
git clone https://github.com/keycloakify/keycloakify-starter
cd keycloakify-starter

cat &#x3C;&#x3C; EOF > ./.dockerignore
node_modules
dist
dist_keycloak
# DO NOT ADD .git and .gitignore
EOF

cat &#x3C;&#x3C; EOF > ./Dockerfile
<strong>FROM node:20-alpine as build
</strong><strong>RUN apk update &#x26;&#x26; \
</strong><strong>    apk add --no-cache git openjdk17 maven
</strong><strong>WORKDIR /app
</strong><strong>COPY . .
</strong><strong>RUN yarn install --frozen-lockfile
</strong><strong>RUN yarn build-keycloak-theme
</strong>
FROM quay.io/keycloak/keycloak:26.0.7
WORKDIR /opt/keycloak
<strong>COPY --from=build /app/dist_keycloak/keycloak-theme-for-kc-all-other-versions.jar /opt/keycloak/providers/
</strong>RUN /opt/keycloak/bin/kc.sh build
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
EOF

docker build -t my-keycloak .
docker run \
    -e KC_BOOTSTRAP_ADMIN_USERNAME=admin \
    -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin \
    -p 8080:8080 \
    my-keycloak
</code></pre>

Ref to official doc: [https://www.keycloak.org/server/containers](https://www.keycloak.org/server/containers)
{% endtab %}

{% tab title="Cloud-IAM" %}
If you have a Keycloak instance managed by [Cloud-IAM](https://cloud-iam.com/?mtm_campaign=keycloakify-deal\&mtm_source=keycloakify-doc-header), you can simply sign-in to and click on the "Upload JAR File" button.
{% endtab %}
{% endtabs %}

## Enabling Your Theme

Once your JAR file is loaded into your Keycloak instance, enable your theme in the Keycloak Admin Console:

1\. Go to **Realm Settings** in your desired realm.

2\. Under the **Themes** section, select your theme from the dropdown menus (e.g., Login Theme).

{% hint style="warning" %}
Never configure the master realm for your application. Create a separate realm for your application to ensure the master realm remains untouched.
{% endhint %}

<figure><img src=".gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

Note that the name that apprear in the dropdown (here "keycloakify-starter") can be configured with [the themeName option](features/compiler-options/themename.md). If you implement [theme variants](features/theme-variants.md) you'll have more than one option.

<details>

<summary>Enabling diffrent theme for diffrent client</summary>

The login theme can be applied at the client level. You typically have one Keycloak client per web application.\
Setting the login theme at the client level means that each application of your realm can have different login/register pages. This comes in handy if you're implementing [Theme Variants](features/theme-variants.md).

To enable a login theme on one of your clients:

* Select your realm in the top left corner
* -> Clients
* -> Select your client in the list
* -> Scroll down to "Login Theme" and select your theme.

The account theme can only be enabled at the realm level; however, accessing the account pages requires authentication. If you don't want your user to inadvertently come across the default login theme when navigating to the account pages after their session has expired, you might want to enable your login theme on the "account-console" client.

* Select your realm in the top left corner
* -> Clients
* -> Select "account console"
* -> Scroll down to "Login Theme" and select one of your login theme.

</details>
