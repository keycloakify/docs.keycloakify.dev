---
icon: diamonds-4
---

# Using a Component Library

The Keycloakify starter repository may initially seem sparse in terms of React/Angular/Svelete components, which might be confusing. However, there's no need to worry—this design choice will soon make sense.

By default, Keycloakify internalizes all the components that make up the default UI, exposing only the `DefaultPage` component.

The idea behind this approach is to let you have in your project only the pages that you have modified and don't overwhelm people that only want to apply CSS customization.

If you want to customize any component from the default theme, you can easily do so by running the following command:

```bash
npx keycloakify eject-page
```

This command allows you to select specific components from Keycloakify's source code, which will then be copied into your own codebase for further customization.

{% embed url="https://youtu.be/PhNE-3EwwP8" %}
Video tutorial on how to use MUI to customize the login page
{% endembed %}

{% hint style="info" %}
Disabling the default styles: One thing that is touched on [only late in the video](https://youtu.be/PhNE-3EwwP8?si=s3e9DjaIlhG2uxQC\&t=1338) is how to disable all the default styles. See documentation [here](../css-customization.md#remove-all-the-default-styles).
{% endhint %}

