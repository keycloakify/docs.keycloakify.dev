# ⬆ v6 -> v7

Many things have changed, I invite you to have a look at the latest version of the starter to see what's up: &#x20;

{% embed url="https://github.com/codegouvfr/keycloakify-starter" %}

The main takeway is that, now your theme source files should be located in a keycloak-theme directory somewhere in your src directory OR at the root of your directory. &#x20;

Acceptable directory strucuture: &#x20;

```
src/
  keycloak-theme/
    login/
    account/
    email/
    
===OR===

src/
  foo/
    bar/
      keycloak-theme/
        login/
        account/
        email/

===OR===

src/
  login/
  account/
  email/
```

You don't need to have all three variant of the theme. If you only need the login theme for example you can have only the `login` directory.\
\
If you had a`keycloak_email_theme` at the root of your project it must be move alongside the `login/` and `account/` directory.

### Step by step migration example

{% embed url="https://github.com/codegouvfr/sill-web/commit/a6a7bfa408fedfacde41946c72c9d055736ab72b" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/871dbcab09f77a57a24eae899e99d163d2d3fb46" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/085e54d4417aa97db1441f28ce978460bceaa7c7" %}

{% embed url="https://github.com/codegouvfr/sill-web/commit/adf5d7779d62f22ec7f617aa0a095865bc72ce96" %}

If you experience issues upgrading to v7 do not hesitate to [ask for help](https://github.com/InseeFrLab/keycloakify/discussions).

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-v6-greater-than-v7" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
