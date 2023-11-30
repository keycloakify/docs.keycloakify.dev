# ✅ Realtime input validation and custom registration fields

{% embed url="https://user-images.githubusercontent.com/6702424/175101958-c2fe36a1-1d78-43ed-999e-92e4b482a0cb.mp4" %}

{% hint style="warning" %}
In reality the regexp used in this gif doesn't work server side, the regexp pattern should be `^[^@]@gmail\.com$` (the RegExp should match the whole string) 😬.
{% endhint %}

User Profile is a Keycloak feature that enables to [define, from the admin console](https://user-images.githubusercontent.com/6702424/136872461-1f5b64ef-d2ef-4c6b-bb8d-07d4729552b3.png), what information you want to collect on your users in the register page and to validate inputs [**on the frontend**, in realtime](https://github.com/InseeFrLab/keycloakify/blob/c4f8879cda657f6c0178b2a7ed01c73c7b7cb5fb/src/login/kcContext/KcContext.ts#L452-L479)!

{% hint style="info" %}
User profile is only available in Keycloak 15 and newer and it's not enabled by default, you have, to start keywloak with the extra parameter:  [`--features=declarative-user-profile`](https://www.keycloak.org/docs/latest/server\_admin/index.html#user-profile)

How you would pass this parameter in practice depend of the Docker image/Helm chart that you are using.\
[Here is an example with docker compose](https://github.com/keycloakify/keycloakify/discussions/292#discussioncomment-5494498).  \
With older Keycloak distribution you can also use JAVA\_OPS environement variable

[docker example](https://user-images.githubusercontent.com/6702424/229278938-fb170876-b848-4362-b125-0f3a19351774.png), [helm example](https://user-images.githubusercontent.com/6702424/229279001-1e35afec-7484-40eb-868e-044a74d684ab.png).\
But the default way, with the official docker image is [this](https://github.com/keycloakify/keycloakify/blob/48cbfc64c07ad92636dd04e04143228a3a53bef2/src/bin/keycloakify/generateStartKeycloakTestingContainer.ts#L57).





The feature also need to be [enabled the feature in the console](https://user-images.githubusercontent.com/6702424/136874428-b071d614-c7f7-440d-9b2e-670faadc0871.png) or you won't see the tab. &#x20;
{% endhint %}

Keycloakify provides client side validation out of the box but for customizing the registration experience you'll have customize `register-user-profile.ftl`

Example in the starter project:

{% embed url="https://github.com/keycloakify/keycloakify-starter/blob/main/src/keycloak-theme/login/pages/RegisterUserProfile.tsx" %}
The RegisterUserProfile page...
{% endembed %}

{% embed url="https://github.com/keycloakify/keycloakify-starter/blob/main/src/keycloak-theme/login/pages/shared/UserProfileFormFields.tsx" %}
...but this is where the magic happens
{% endembed %}

#### Password validation

You can establish guidelines for what constitutes a valid password, such as a minimum length, by utilizing the Keycloak Admin UI under 'Authentication -> Password Policy'. However, as of now, [Keycloak does not support front-end validation of passwords](https://keycloak.discourse.group/t/make-password-policies-available-to-freemarker/11632).

This limitation means that we can't provide immediate feedback to the user on the client side about the validity of their chosen password. Only after the user clicks on 'Register' will they receive an error if the chosen password does not adhere to the server-defined password policy.

Should you insist on implementing client-side password validation, [you can pass validators to the useFormValidation function](https://github.com/keycloakify/keycloakify-starter/blob/25d66046d6a364b934a5436e7a14be013d2124de/src/keycloak-theme/login/pages/shared/UserProfileFormFields.tsx#L29-L38), although it's not typically recommended. The reason for this is potential confusion for users if the validator defined in your theme does not align with the server's password policy.

### Getting your custom user attribute to be included in the JWT &#x20;

In [the public deployment of the starter project](https://starter.keycloakify.dev/).&#x20;

If you register you'll see that you have to select if your are a cat or dog person. &#x20;

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

This appear because, on my keycloak server I have configured a custom user profile attribute.  \


<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

And when the user is connecte, it's choice appear in the JWT &#x20;

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

This work because I have created the following mapper in my Keycloak configuration pannel:&#x20;

Go to clients -> [starter (name of your client)](https://github.com/keycloakify/keycloakify-starter/blob/5689455bc040442c9d71356cc0ce1be28c301f02/src/App/App.tsx#L16) -> Mappers

* Name: `Favourite pet`
* Mapper type: `User attribute`
* User attribute: `favourite_pet`
* Token claim name: `favourite_pet`
* Claim JSON type: `string`

{% embed url="https://cloud-iam.com/?mtm_campaign=keycloakify-deal&mtm_source=keycloakify-doc-realtime-input-validation" %}
Feeling overwhelmed? Check out our exclusive sponsor's Cloud IAM consulting services to simplify your experience.
{% endembed %}
