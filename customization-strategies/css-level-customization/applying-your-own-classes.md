# Applying your own classes

{% hint style="info" %}
If you're porting a pre-existing, non Keycloakify theme to Keycloakify this is the approach you would implement.
{% endhint %}

If you have an utility stylesheet that defines standardized classes you can apply them using the class object like so:

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Result:

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
If you want to implement this approach with **Tailwind** read [this](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/KcPage.tsx#L53-L65). But there's a section dedicated to Tailwind [here](using-tailwind.md).
{% endhint %}

Up next:

{% content-ref url="page-specific-styles.md" %}
[page-specific-styles.md](page-specific-styles.md)
{% endcontent-ref %}
