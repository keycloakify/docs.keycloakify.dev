---
description: Only allow specific emails to register.
---

# 💂 Email domain acceptlist

{% embed url="https://user-images.githubusercontent.com/6702424/164943571-560cdccb-2c68-40b8-bd45-c229401f3fd4.mp4" %}
Can't register with a @gmail.com address
{% endembed %}

{% tabs %}
{% tab title="With user profile" %}
Using user profile capabilities of keycloak you can define a Regexp on the email field that will only allow certain domains to register.

The Regexp should look [something like this](https://github.com/InseeFrLab/onyxia/blob/fee2b51d0c7ce43d4d25c5c16530a65e71662bd8/web/src/keycloak-theme/login/kcContext.ts#L39) and can be generated easily with [this helper](https://github.com/InseeFrLab/onyxia/blob/v8.20.2/web/scripts/emails-domain-accept-list-helper.ts).

You can find [here](https://github.com/InseeFrLab/onyxia/blob/fee2b51d0c7ce43d4d25c5c16530a65e71662bd8/web/src/keycloak-theme/login/pages/shared/UserProfileFormFields.tsx#L204-L223) the source code that allow to convert the regexp back into an array of string to display a tooltip that show what emails are allowed like shown on the video above. &#x20;
{% endtab %}

{% tab title="With a third party Keycloak plugin" %}
The legacy way of doing it would be with [this plugin](https://github.com/micedre/keycloak-mail-whitelisting) and using`kcContext["authorizedMailDomains"]` to validate in realtime.

Relevant code [here](https://github.com/garronej/keycloakify-demo-app/blob/a316ea0046976e6d435a33e896cb9e3d1873c124/src/KcApp/kcContext.ts#L11) and [here](https://github.com/garronej/keycloakify-demo-app/blob/a316ea0046976e6d435a33e896cb9e3d1873c124/src/KcApp/kcContext.ts#L35-L41).
{% endtab %}
{% endtabs %}

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-email-domain-acceptlist" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud-IAM consulting services to simplify your experience.
{% endembed %}
