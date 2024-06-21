---
description: Consent page
---

# 📄 Terms and conditions

The Tems and Condition feature of Keycloak enalbes you to make new users of your service consent to the terms of use of your services upon regisering. &#x20;

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Enabling in Keycloak

If you want you show your terms and condition page when they create an account you have to enable it in your realm configuration. &#x20;

This is how to do it in the Keycloak Admin Console: &#x20;

TODO: Tango

## Defining your Terms and Conditions text

There are multiple strategies for providing and updating the therms of conditions of your services pick the one that best fit your usecase. &#x20;

### Using Keycloak Realm Configuration

The default way of defining your terms of services in Keycloak is to provide a message bundle for your realm that overrides the `temsText` key for the different language that are enabled in your realm. &#x20;

In recent Keycloak's versions this can be acheived directly via the Keycloak Admin Console as shown in this video. &#x20;

{% embed url="https://youtu.be/naW2TxwJZsA" %}

### Using Markdown Text



