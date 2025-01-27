# Outside of Keycloak

The recommended way to preview your theme as you develop it is to use [Storybook](https://storybook.js.org/).\
Storybook is a tool that enables to test UI component in isolation. For reference the following website was generated with storybook:

{% embed url="https://storybook.keycloakify.dev/" %}

{% hint style="info" %}
If you prefer to avoid intoducing Storybook into your stack, it's okay, you can still preview your page in dev mode. Do do so, refer to [this guide](outside-of-keycloak-without-storybook.md).
{% endhint %}

The starter template does not initially contain any story files, instead there's a keycloakify CLI command that let's you import specifically the stories for the pages you want to test into your project.

So, just run this command in the root of your Keycloakify project and select the pages you want.

```bash
npx keycloakify add-story
```

It will enables you to select the pages you want to add stories for.

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

Selecting login -> register.ftl will result in this file to be created in your project:

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

You can run the above command multiple times to add stories for the different pages you want to develop.

Once your added a few stories you can start Storybook locally with:

```bash
npm run storybook
```

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

You can see the changes you make in you code in realtime in your Storybook.

The idea of Storybook is to easily let you see the pages in different configuration without having to reproduce the full login/register process in a real Keycloak.\
Keycloakify provide a default mock context for every pages, the stories let you partially override some specific part of this default mock to reflect pages in different configurations.\
\
For example, if you want to create a story that show the register page in chinese you would add this:

{% tabs %}
{% tab title="React" %}
<pre class="language-tsx" data-title="src/login/pages/Register.stories.tsx"><code class="lang-tsx">import type { Meta, StoryObj } from "@storybook/react";
import { createKcPageStory } from "../KcPageStory";

const { KcPageStory } = createKcPageStory({ pageId: "register.ftl" });

const meta = {
    title: "login/register.ftl",
    component: KcPageStory
} satisfies Meta&#x3C;typeof KcPageStory>;

export default meta;

type Story = StoryObj&#x3C;typeof meta>;

export const Default: Story = {
    render: () => &#x3C;KcPageStory />
};

<strong>export const InChinese: Story = {
</strong><strong>    render: ()=> (
</strong><strong>        &#x3C;KcPageStory
</strong><strong>            kcContext={{
</strong><strong>                locale: {
</strong><strong>                    currentLanguageTag: "zh-CN"
</strong><strong>                }
</strong><strong>            }}
</strong><strong>        />
</strong><strong>    )
</strong><strong>};
</strong></code></pre>


{% endtab %}

{% tab title="Angular" %}
{% code title="src/login/pages/register/register.stories.ts" %}
```typescript
import { Meta, StoryObj } from '@storybook/angular';
import { decorators, KcPageStory } from '../KcPageStory';
import { Attribute } from 'keycloakify/login';

const meta: Meta<KcPageStory> = {
    title: 'login/register.ftl',
    component: KcPageStory,
    decorators: decorators,
    globals: {
        pageId: 'register.ftl'
    }
};

export default meta;

type Story = StoryObj<KcPageStory>;

export const Default: Story = {};

export const InChinese: Story = {
    globals: {
        kcContext: {
            locale: {
                    currentLanguageTag: "zh-CN"
            }
        }
    }
};
```
{% endcode %}


{% endtab %}

{% tab title="Svelte" %}
{% code title="src/login/pages/Login.stories.svelte" %}
```html
<script
  context="module"
  lang="ts"
>
  import { defineMeta } from '@storybook/addon-svelte-csf';
  import type { KcPageStoryProps } from '../KcPageStory';
  import KcPageStory from '../KcPageStory.svelte';

  const args: KcPageStoryProps = { pageId: 'login.ftl' };
  const { Story } = defineMeta({
    title: 'login/login.ftl',
    component: KcPageStory,
    args: args,
  });
</script>

<Story name="Default" />

<Story
  name="InChinese"
  args={{
    ...args,
    kcContext: {
      locale: {
        currentLanguageTag: "zh-CN"
      }
    }
  }}
/>
```
{% endcode %}
{% endtab %}
{% endtabs %}



<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

Now let's see how to test our theme in a real Keycloak instance:

{% content-ref url="inside-of-keycloak.md" %}
[inside-of-keycloak.md](inside-of-keycloak.md)
{% endcontent-ref %}
