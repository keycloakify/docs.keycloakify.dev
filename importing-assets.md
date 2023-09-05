# 🖼 Importing assets

If you are familiar with the React ecosystem already, just import assets like you are used to, it should work out of the box.  \
For the others, or thoses who want to see precisely what's supported here are some instructions.&#x20;

### Importing custom assets

If you want to import your own, images, video, anything you should put them anywhere under src/ and import them like you would import code; &#x20;

Example you can put foo.png in src/keycloak-theme/login/assets and import it like this in the template:&#x20;

{% code title="Template.tsx" %}
```tsx
import fooPngUrl from "./assets/foo.png";
// NOTE: fooPngUrl is a url (string) that point to the asset. 

//...


<img src={fooPngUrl} />
```
{% endcode %}

Note that this isn't a feature of Keycloakify, Create React App, Vite and Next.js all support importing assets this way.  \


Now there is one thing that is supported in vanila Create React App project but you can't do when using Keycloakify: placing assets in the public/ directory and importing them manually. &#x20;

Like putting foo.png into public/img/foo.png and importing it like:

{% code title="Template.tsx" %}
```tsx
// This work in CRA but does not when using Keycloakify
<img src={process.env.PUBLIC_URL + "img/foo.png"} />
// NOTE: process.env.PUBLIC_URL is usualy the empty string
```
{% endcode %}

This way of importing assets however is worth both in term of performance and maintainability so it shouldn't be a probelm that it isn't supported. &#x20;

### Importing default theme resources

{% hint style="info" %}
This section is more for transparency sake than anything. \
You should use your own assets inseted of importing the default ones, this is the all point of creating a custom theme. &#x20;
{% endhint %}

If you want to import the resources that are available in the default theme you can construct urls like: &#x20;

```tsx
const paternlyflyUrl = `${kcContext.url.resourcesCommonPath}/node_modules/patternfly/dist/css/patternfly.min.css`;
const loginCss = `${url.resourcesPath}/css/login.css`;
```

You can see what assets are available in public/keycloak-resources/login/resources.  \
If you want to chose what version of the assets are available you can use the [this build option](build-options.md#loginthemeresourcesfromkeycloakversion). &#x20;

By default the default css assets are imported and applyed [here](https://github.com/keycloakify/keycloakify/blob/402c6fc64a26268b6f2f7222e4f11ff07de452f8/src/login/Template.tsx#L35-L38C19) in the login theme.&#x20;
