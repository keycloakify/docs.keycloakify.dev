# In Storybook

{% hint style="info" %}
TLDR:

```bash
npx keycloakify add-story
npm run storybook
```
{% endhint %}

[Storybook](https://storybook.js.org/) is a tool that enables to test UI component in isolation. For reference, the component showcase Keycloakify website is a website generated with Storybook.

{% embed url="https://storybook.keycloakify.dev/?path=/story/introduction--page" %}

The starter template does not initially contain any story files, instead there's a keycloakify CLI command that let's you import specifically the stories for the pages you want to test into your project.&#x20;

So, just run this command in the root of your Keycloakify project and select the pages you want. &#x20;

```bash
npx keycloakify add-stories
```

It will enables you to select the pages you want to add stories for.&#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Selecting login -> register.ftl will result in this file to be created in your project:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You can run the above command multiple times to add stories for the different pages you want to develop.

Once your added a few stories you can start Storybook locally with:

```bash
npm run storybook
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You can see the changes you make in you code in realtime in your Storybook. &#x20;

The idea of Storybook is to easyly let you see the pages in different configuration without having to reproduce the full login/register cinematic in a real Keycloak.  \
Keycloakify provide a default mock context for every pages, the stories let you partially overrride some specific part of this default mock to reflect pages in different configurations.  \
\
For example, if you want to create a story that show the register page in chinese you would add this:

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

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

That's really nice, however this approach has it's limits. At some point you'll want to test in a real Keycloak to make sure everything works. Also you don't nessesary know the kcContext values to provides in order to replicate a desired configuration. &#x20;

Don't worry! Keycloakify got you covered by letting you test in a local Keycloak Docker container.

{% content-ref url="in-a-keycloak-docker-container.md" %}
[in-a-keycloak-docker-container.md](in-a-keycloak-docker-container.md)
{% endcontent-ref %}
