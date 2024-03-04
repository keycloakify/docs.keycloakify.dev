# 🌉 Context persistence

Let's explore how we can pass query params to the URL before redirecting to the login page so that we can transport some state from the main app to the login page.

It's up to you to implement it however you want but here's a solution off the shelf for you to use

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/main/src/keycloak-theme/login/valuesTransferredOverUrl.ts" %}
Declare the variables that we want to pass over, here foo and bar
{% endembed %}

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/cb5844c62381efed7b303886cbe460c055a62c21/src/App/App.tsx#L21-L27" %}
Add the value to the url before redirecting
{% endembed %}

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/cb5844c62381efed7b303886cbe460c055a62c21/src/keycloak-theme/login/KcApp.tsx#L10-L12" %}
Using the values in the login pages
{% endembed %}

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-context-persistence" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud-IAM consulting services to simplify your experience.
{% endembed %}
