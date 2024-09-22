# Basic example

As you can see in the screenshot below most DOM Element get assigned a class starting by kcSomething. Example kcFormHeaderClass.

No styles rules get assigned to those classes they are only here for you to use as target for your custom CSS.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Inspecting the login.ftl page in chrome dev tools</p></figcaption></figure>

So if you're not very interested in all the bells and whistles Keycloakify offers, you can just create a CSS[^1] file and just start customizing the page:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Applying a red border to all the DOM element with the kcFormHeaderClass class</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="375"><figcaption><p>The red border gets applied</p></figcaption></figure>

Up next:

{% content-ref url="removing-the-default-styles.md" %}
[removing-the-default-styles.md](removing-the-default-styles.md)
{% endcontent-ref %}

[^1]: ...or LESS or SASS
