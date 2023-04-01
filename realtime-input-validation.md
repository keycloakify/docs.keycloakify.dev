# ✅ Realtime input validation and custom registration fields

{% embed url="https://user-images.githubusercontent.com/6702424/175101958-c2fe36a1-1d78-43ed-999e-92e4b482a0cb.mp4" %}

{% hint style="warning" %}
In reality the regexp used in this gif doesn't work server side, the regexp pattern should be `^[^@]@gmail\.com$` (the RegExp should match the whole string) 😬.
{% endhint %}

User Profile is a Keycloak feature that enables to [define, from the admin console](https://user-images.githubusercontent.com/6702424/136872461-1f5b64ef-d2ef-4c6b-bb8d-07d4729552b3.png), what information you want to collect on your users in the register page and to validate inputs [**on the frontend**, in realtime](https://github.com/InseeFrLab/keycloakify/blob/c4f8879cda657f6c0178b2a7ed01c73c7b7cb5fb/src/login/kcContext/KcContext.ts#L452-L479)!

{% hint style="info" %}
User profile is only available in Keycloak 15 and newer and it's not enabled by default, you have, to enable it set:  &#x20;

`JAVA_OPTS=-Dkeycloak.profile=preview` ([docker example](https://user-images.githubusercontent.com/6702424/229278938-fb170876-b848-4362-b125-0f3a19351774.png), [helm example](https://user-images.githubusercontent.com/6702424/229279001-1e35afec-7484-40eb-868e-044a74d684ab.png))&#x20;

or &#x20;

`KEYCLOAK_EXTRA_ARGS: --features=declarative-user-profile` ([helm example](https://github.com/keycloakify/keycloakify/discussions/292#discussioncomment-5494498))&#x20;



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

{% hint style="warning" %}
As of today [Keycloak dosen't allow to define a pattern for the password](https://keycloak.discourse.group/t/make-password-policies-available-to-freemarker/11632) in the admin console. You can however pass validators for it to the `useFormValidation` function. (this is why useFormValidation returns `attributesWithPassword`)
{% endhint %}

### Getting your custom user attribute to be included in the JWT &#x20;

In [the public deployment of the starter project](https://starter.keycloakify.dev/).&#x20;

If you register you'll see that you have to select if your are a cat or dog person. &#x20;

<figure><img src=".gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

This appear because, on my keycloak server I have configured a custom user profile attribute. &#x20;

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

And when the user is connecte, it's choice appear in the JWT &#x20;

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

This work because I have created the following mapper in my Keycloak configuration pannel:&#x20;

Go to clients -> [starter (name of your client)](https://github.com/keycloakify/keycloakify-starter/blob/5689455bc040442c9d71356cc0ce1be28c301f02/src/App/App.tsx#L16) -> Mappers

* Name: `Favourite pet`
* Mapper type: `User attribute`
* User attribute: `favourite_pet`
* Token claim name: `favourite_pet`
* Claim JSON type: `string`
