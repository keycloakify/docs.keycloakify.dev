# 🖇️ Integration With Custom Keycloak Extention

Let's say for example that you have a custom Keycloak extention enabled in your Keycloak server that implement SMS OTP and features a custom user facing page "sms-otp.ftl".  \
\
You can implement this custom page in your Keycloakify project. Here is a branch of the starter to demonstrate this process: &#x20;

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/custom-keycloak-extention-page" %}
Branch of the starter where with a custom sms-otp.ftl page
{% endembed %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/2e81a70ac44856b425335b70ca042d3bcd8c209d" %}
Diff view of the changes to add the page
{% endembed %}

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Keycloakify generates a sms-otp.ftl page</p></figcaption></figure>
