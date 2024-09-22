# 🔛 Enabling your Theme in the Keycloak Admin Console

Let's see how to enable your theme once you have sucessfully imported it in your Keycloak instance.

<figure><img src=".gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

## Enabeling globaly on your realm

{% hint style="warning" %}
In any senario you should never use the Keycloak reserved realm (master) for your application.\
You should create one.
{% endhint %}

The first options is to enable the your themes at the realm level, which mean every applications that uses this realm will get this theme applied.

* Select your realm it the top left corner
* \-> Realm settings
* \-> "Themes" tab

Here you'll be able to select your login, account and email theme.

## Enabling a theme for a specific client

The login theme can be applied at the client level. You have typically one Keycloak client per web applications.  \
Setting the login theme at the client level means that each application of your realm can have different login/register pages. This comes in handy if you're implementing [Theme Variants](theme-variants.md).

To enable a login theme on one of your client:

* Select your realm in the top left corner
* \-> Clients
* \-> Select your client in the list
* \-> Scroll down to "Login Theme" and select your theme.

The account theme can only be enabled at the realm level; however, accessing the account pages requires authentication. If you don't want your user to inadvertently come across the default login theme when navigating to the account pages after their session has expired, you might want to enable your login theme on the "account-console" client.

* Select your realm in the top left corner
* \-> Clients
* \-> Select "account console"
* \-> Scroll down to "Login Theme" and select one of your login theme.
