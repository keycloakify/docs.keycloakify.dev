---
description: >-
  If you want to overwrite the translation messages that comes by default with
  Keycloak, define some new messages, or add translation for a new language.
---

# 🌎 i18n: msg(...)

{% hint style="info" %}
In Keycloakify you don't [directly edit the `messages_xx.properties` files](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FsspJ8BvaNa5VrAWRnnD0%2Fuploads%2FARZ2fA82vANcrQ30kEac%2FUntitled.png?alt=media\&token=14c35c9a-e78d-4cf0-9037-22097eb6071b).
{% endhint %}

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/main/src/keycloak-theme/login/i18n.ts" %}
Overwiting default messages or defining new ones
{% endembed %}

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/cb5844c62381efed7b303886cbe460c055a62c21/src/keycloak-theme/login/KcApp.tsx#L37-L46" %}
Using the i18n API
{% endembed %}

{% hint style="success" %}
You can set the language you'll get in `i18n.curentLanguageTag` by specifying `ui_locales=xx` as query parameter when redirecting to your login or register page.

[See how](context-persistence.md).
{% endhint %}

{% embed url="https://www.cloud-iam.com/" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
