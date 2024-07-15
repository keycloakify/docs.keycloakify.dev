# In a Keycloak Docker Container

{% hint style="info" %}
TLDR:

```bash
npx keycloakify start-keycloak
```
{% endhint %}



Testing your theme in Storybook is nice, but at some point you'll want to test your theme in a real Keycloak before shipping it in production!

First you want to install and launch [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or just Docker) on your computer, if you haven't done it already. &#x20;

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
You are ready! In your Keycloakify just run:&#x20;

```bash
npx keycloakify start-keycloak
```

You'll be invited to chose the Keycloak version you want to spin up:

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Keycloakify will preconfigure a realm and client for your theme so you don't necessarily need to go in the the Keycloak admin console you can simply lavigate to **https://my-theme.keycloakify.dev** it will redirect to your local Keycloak login pages! &#x20;

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
If you makes changes in your theme while the Keycloak container is running your theme will be automatically recompiled and updated in Keycloak. After a few seconds you'll just have to refresh the page you see them live. &#x20;
{% endhint %}

You'll be able to authenticate with the test user:

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

And see the content of your Open ID Connect Access Tocken JWT:

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Here you can chose to go back to the login page or you have a link to the account pages:

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Now what's interesting is you can see the real kcContext in the dev tools. You can use it as a reference to create new stories!

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

If you connect to the Keycloak Admin UI (**http://localhost:8080**) and make some changes to the Ream configuration you can then export theses change into a .json file using the partial export feature of Keycloak:

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption><p>When exporting, enable "export clients"</p></figcaption></figure>

Later on, you can then start another container again with the same configuration using:

```bash
npx keycloakify start-keycloak --import realm-export.json
```
