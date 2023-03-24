---
description: Customize the default email template
---

# 📧 Email customization

_Introduced in_ [_v4.8.0_](https://github.com/InseeFrLab/keycloakify/releases/tag/v4.8.0)

It is now possible to customize the emails sent to your users to confirm their email address ect.\
Just run `npx initialize-email-theme`, it will create a `src/keycloak-theme/email` directory at the root of your project.\
This directory should be tracked by Git (`yarn add -A`) You can start hacking the default template.

You can remove all the template and resource file you aren't going to customize (in this case it will fallback to the default email theme).  \
When `npx build-keycloak-theme` (`yarn keycloak`) is run it will bundle your email theme into your `.jar` file and you will be able to select it in the Keycloak administration pages.

![Selecting your email theme in the Keycloak admin](.gitbook/assets/email.png)
