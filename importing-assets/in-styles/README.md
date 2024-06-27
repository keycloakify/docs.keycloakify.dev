# In Styles

Let's see, as an example, the different ways you have to change the backgrouns image of the login page. &#x20;

First let's [download a background image](https://coolbackgrounds.io/) an put it in our public directory:

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to do so, you can hot swipe assets that you have placed into your public directory in your Keycloak instance files at:

**/opt/keycloak/themes/**[**\<name of your theme>**](../../build-options/themename.md)**/\<login|account>/resources/dist**

![](<../../.gitbook/assets/image (28).png>)
{% endhint %}

Now there's three main cathegory of Styling solution, pick the guide for your favorite one:

{% tabs %}
{% tab title="Plain .css files" %}
{% content-ref url="plain-css.md" %}
[plain-css.md](plain-css.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="CSS in JS" %}
{% content-ref url="css-in-js.md" %}
[css-in-js.md](css-in-js.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="Tailwind" %}
{% content-ref url="tailwind.md" %}
[tailwind.md](tailwind.md)
{% endcontent-ref %}
{% endtab %}
{% endtabs %}
