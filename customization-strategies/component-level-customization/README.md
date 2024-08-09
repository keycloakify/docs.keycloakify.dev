# Component Level Customization

The Keycloakify starter repository may initially seem sparse in terms of React components, which might be confusing. However, there's no need to worry—this design choice will soon make sense.

By default, Keycloakify internalizes all the React components that make up the default theme, exposing only the `<DefaultPage />` component.

If you want to customize any component from the default theme, you can easily do so by running the following command:

```bash
npx keycloakify eject-page
```

This command allows you to select specific components from Keycloakify's source code, which will then be copied into your own codebase for further customization.

{% embed url="https://youtu.be/PhNE-3EwwP8" %}
Video tutorial on how to use MUI to customize the login page
{% endembed %}

Following is a step by step guide on how to import and use custom assets in your react components:

{% content-ref url="in-react-components.md" %}
[in-react-components.md](in-react-components.md)
{% endcontent-ref %}
