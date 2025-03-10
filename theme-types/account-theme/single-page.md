---
description: Customizing the Single Page Account UI
---

# Single-Page

To initialize a Single-Page account theme run the following command:

```bash
npx keycloakify initialize-account-theme # Select 'Sigle-Page'
```

{% embed url="https://youtu.be/UKU6zGCH-CY" %}
Video Tutorial
{% endembed %}

📌 Timestamps:\
[00:00](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=0s) – Intro\
[03:33](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=213s) – Changing the logo\
[07:46](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=466s) – Using a custom button component\
[14:13](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=853s) – Update process\
[16:55](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=1015s) – Translations (i18n)\
[18:23](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=1103s) – Enabling your account theme in the Keycloak Admin Console\
[19:55](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=1195s) – Admin Theme

### Adding custom CSS (not touched upon in the video)

The official way of customizing the look of the Account UI is to overload the PaternFly CSS variables:&#x20;

{% embed url="https://www.patternfly.org/components/button/html/#css-variables" %}

But we are aware that this isn't enough for most of you. &#x20;

Beyond that you can of course import your custom CSS and use the PaternFly utility classes as target but be aware that theses classes can be changed in the future.

\
Example loading some custom CSS:

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption><p>The red boder has been applied on all the element that have the pf-v5-c-page__main-secrion class</p></figcaption></figure>

