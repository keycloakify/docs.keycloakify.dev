# 🖼️ Importing Assets

{% tabs %}
{% tab title="Vite" %}
{% hint style="info" %}
TLDR: There is nothing specific to Keycloakify about importing assets. You can do it however you would in any other project.

Just if you're referencing assets that are in the public directory, use `import.meta.env.BASE_URL`
{% endhint %}
{% endtab %}

{% tab title="Webpack" %}
{% hint style="info" %}
TLDR:  You can import asset like you would in any other project, one exception being: If you reference assets that are located in your public directory from within your TSX files you must use Keycloakify's polifill of the `PUBLIC_URL` environnement variable, you can't use `process.env.PUBLIC_URL` directly:

```tsx
import { PUBLIC_URL } from "keycloakify/PUBLIC_URL";
<img src={`${PUBLIC_URL}/my-image.png`} />
```
{% endhint %}
{% endtab %}
{% endtabs %}

{% content-ref url="in-your-components.md" %}
[in-your-components.md](in-your-components.md)
{% endcontent-ref %}

{% content-ref url="in-css.md" %}
[in-css.md](in-css.md)
{% endcontent-ref %}
