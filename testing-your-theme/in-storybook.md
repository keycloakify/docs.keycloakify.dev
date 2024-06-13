# In Storybook

Storybook is a tool that enables to test UI component in isolation. Keycloakify provide an integration with this tool and is the preffed way to develope your theme.  \
\
First step is to go over the Keycloakify component website and identify the pages that you want to test. &#x20;

{% embed url="https://docs.keycloakify.dev/" %}

Then simply run the following command in your Keycloakfiy project:

```bash
npx keycloakify add-stories
```

It will enables you to select the pages you want to add stories for. \
Finally you can start with:

```bash
yarn storybook
```
