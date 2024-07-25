# 🖇️ Integration With Custom Keycloak Extension

Let's say for example that you have a custom Keycloak extension enabled in your Keycloak server that implement SMS OTP and features a custom user facing page "sms-otp.ftl".\
\
You can implement this custom page in your Keycloakify project. Here is a branch of the starter to demonstrate this process:

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/custom-keycloak-extension-page" %}
Branch of the starter where with a custom sms-otp.ftl page
{% endembed %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/fec0e9290d6820e5604150a478cce63a500e228f" %}
Diff view of the changes to add the page
{% endembed %}

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Keycloakify generates a sms-otp.ftl page</p></figcaption></figure>
