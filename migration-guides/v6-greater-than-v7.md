# ⬆ v6 -> v7

Many things have changed, I invite you to have a look at the latest version of the starter to see what's up: &#x20;

{% embed url="https://github.com/codegouvfr/keycloakify-starter" %}

### Step by step migration example

{% embed url="https://github.com/codegouvfr/sill-web/commit/a6a7bfa408fedfacde41946c72c9d055736ab72b" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/871dbcab09f77a57a24eae899e99d163d2d3fb46" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/085e54d4417aa97db1441f28ce978460bceaa7c7" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/adf5d7779d62f22ec7f617aa0a095865bc72ce96" %}

### Email theme

The email theme should no longer be at the root of your project in a `keycloak_email_theme` directory.  \
It should be moved to  `keycloak-theme/email`. The `keycloak-theme` dir can be anywhere in your src directory. In the starter project it's at `src/keycloak-theme`

If you experience issues upgrading to v7 do not hesitate to [ask for help](https://github.com/InseeFrLab/keycloakify/discussions).

{% embed url="https://www.cloud-iam.com/" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
