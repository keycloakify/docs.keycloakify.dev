# 🚛 Passing URL Parameters to your Theme

Let's explore how we can pass query params to the URL before redirecting to the login page so that we can transport some values from the main app to the login page.

{% embed url="https://github.com/keycloakify/keycloakify-starter/blob/0c56eff3b00b99fd723de1dcdb91c40a3b3478cd/src/App/oidc.ts#L23" %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/blob/0c56eff3b00b99fd723de1dcdb91c40a3b3478cd/src/keycloak-theme/login/pages/Login.tsx#L9-L13" %}

You might want to store the value in the local storage if otherwise you'll lost it when the user navigate from the login page to the register page. Example implementation [here](https://github.com/InseeFrLab/onyxia/blob/40d393973398f5bbcea60d7cd9a9a9e0267bd273/web/src/keycloak-theme/login/onyxiaInstancePublicUrl.ts#L6-L28).
