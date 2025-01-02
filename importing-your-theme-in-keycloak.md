# 📥 Importing the JAR of Your Theme Into Keycloak

Now that you have your theme as a .jar file, let's see how you can import it in Keycloak so that it appears in the dropdown list for selecting theme in the Keycloak Admin console.

<figure><img src=".gitbook/assets/image (85).png" alt="" width="375"><figcaption><p>Custom login and account theme selected in the Keycloak Admin console</p></figcaption></figure>

{% hint style="warning" %}
Takes the following instructions with a grain of salt.  \
They are contributed by the comunity and might not be up to date.  \
They'll give you an idea of how it's suposed to be done but you'd be better of looking for the official documentation on how to load keycloak extentions. &#x20;
{% endhint %}

{% tabs %}
{% tab title="Docker" %}
<pre class="language-sh"><code class="lang-sh">cd ~/github
git clone https://github.com/keycloakify/keycloakify-starter
cd keycloakify-starter
# Just to make sure these instructions remain relevant in the future
# We pin the version of the starter we are using.  
git checkout c6511feee3d9471f6ea67bc5176e28150ab951ef
yarn
yarn build-keycloak-theme

docker run \
    -p 8080:8080 \
    --name my-keycloak \
    -e KC_BOOTSTRAP_ADMIN_USERNAME=admin \
    -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin \
<strong>    -v "./dist_keycloak/keycloak-theme-for-kc-all-other-versions.jar":/opt/keycloak/providers/keycloak-theme.jar \
</strong>    quay.io/keycloak/keycloak:26.0.4 \
    start-dev
</code></pre>

{% hint style="warning" %}
Here we use `"start-dev"` but in production use `"start --optimized"`
{% endhint %}
{% endtab %}

{% tab title="Docker - Custom Image" %}
Let's see how you would go about creating a Keycloak Docker image with your theme available.

{% embed url="https://willwill96.github.io/the-ui-dawg-static-site/en/keycloakify/#integrating-keycloak-and-keycloakify-jar" %}
Checkout this great tutorial that explains it in great details
{% endembed %}

<pre class="language-bash"><code class="lang-bash">cd ~/github
mkdir docker-keycloak-with-theme
cd docker-keycloak-with-theme
git clone https://github.com/keycloakify/keycloakify-starter
cd keycloakify-starter
# Just to make sure these instructions remain relevant in the future
# We pin the version of the starter we are using.  
git checkout c6511feee3d9471f6ea67bc5176e28150ab951ef
cd ..

cat &#x3C;&#x3C; EOF > ./Dockerfile
FROM node:18 as keycloakify_jar_builder
RUN apt-get update &#x26;&#x26; \
    apt-get install -y openjdk-17-jdk &#x26;&#x26; \
    apt-get install -y maven;
COPY ./keycloakify-starter/package.json ./keycloakify-starter/yarn.lock /opt/app/
WORKDIR /opt/app
RUN yarn install --frozen-lockfile
COPY ./keycloakify-starter/ /opt/app/
RUN yarn build-keycloak-theme

FROM quay.io/keycloak/keycloak:26.0.4 as builder
WORKDIR /opt/keycloak
<strong>COPY --from=keycloakify_jar_builder /opt/app/dist_keycloak/keycloak-theme-for-kc-all-other-versions.jar /opt/keycloak/providers/
</strong>RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:latest
COPY --from=builder /opt/keycloak/ /opt/keycloak/
ENV KC_HOSTNAME=localhost
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev"]
EOF

docker build -t docker-keycloak-with-theme .
docker run \
    -e KC_BOOTSTRAP_ADMIN_USERNAME=admin \
    -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin \
    -p 8080:8080 \
    docker-keycloak-with-theme
</code></pre>

{% hint style="warning" %}
In this Docker file we use `ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev"]` but in production use `ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]`
{% endhint %}
{% endtab %}

{% tab title="Docker Compose" %}
Let's assume that you have the following directory structure: &#x20;

```
./docker-compose.yaml
./themes/keycloak-theme-for-kc-all-other-versions.jar # This is the file generated in `dist_keycloak` when running `yarn build-keycloak-theme` 
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
    version: 21.4.1 # Keycloak 24
    repository: oci://registry-1.docker.io/bitnamicharts
```
{% endcode %}

Here we only list the relevant values:

<pre class="language-yaml" data-title="values.yaml"><code class="lang-yaml">keycloak:
  initContainers:
    - name: realm-ext-provider
      image: curlimages/curl
      imagePullPolicy: IfNotPresent
      command:
        - sh
      args:
        - -c
        - |
<strong>          # Replace USER and PROJECT, use the correct version of the jar for the keycloak version you are deploying    
</strong><strong>          curl -L -f -S -o /extensions/keycloak-theme.jar https://github.com/USER/PROJECT/releases/download/v11.3.16/keycloak-theme-for-kc-22-to-25.jar
</strong>
      volumeMounts:
        - name: extensions
          mountPath: /extensions

  extraVolumeMounts:
    - name: extensions
      mountPath: /opt/bitnami/keycloak/providers

  extraVolumes:
    - name: extensions
      emptyDir: {}
</code></pre>

Read [this section of the starter project readme](https://github.com/keycloakify/keycloakify-starter?tab=readme-ov-file#github-actions) to learn how to get GitHub Action to publish your theme's JAR as assets of your GitHub release.
{% endtab %}

{% tab title="Bare metal" %}
What you need to know is that your keycloak-theme.jar should be placed in the provider directory of your Keycloak (e.g: `/opt/keycloak/providers)`\
After that you should run bin/kc.sh build (e.g: `bash /opt/keycloak/bin/kc.sh build`)

Then you can start your Keycloak server, your theme should be available in it!
{% endtab %}

{% tab title="Cloud-IAM" %}
If you are utilizing a Keycloak instance managed by [Cloud-IAM](https://cloud-iam.com/?mtm_campaign=keycloakify-deal\&mtm_source=keycloakify-doc-header), importing themes and extensions is quite straightforward.

{% hint style="info" %}
Uploading custom JAR files is only available with paid plans.

If you decide to subscribe, please consider using the code `keycloakify5`.

This code will provide you with a 5% discount, and we will also receive 5%, which greatly supports our project!
{% endhint %}

{% embed url="https://app.tango.us/app/embed/e22aec8f-d7cf-44a7-bfe7-9f92630aa7eb" %}
{% endtab %}
{% endtabs %}
