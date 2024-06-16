# In Storybook

Storybook is a tool that enables to test UI component in isolation. Keycloakify provide an integration with this tool and is the preffed way to develope your theme.  \
\
First step is to go over the Keycloakify component website and identify the pages that you want to test. &#x20;

{% embed url="https://docs.keycloakify.dev/" %}

The starter template does not initially contain any story files, instead there's a keycloakify CLI command that let's you import specifically the stories for the pages you want to test into your project.&#x20;

So, just run this command in the root of your Keycloakify project and select the pages you want. &#x20;

```bash
npx keycloakify add-stories
```

It will enables you to select the pages you want to add stories for.&#x20;

You can run this command multiple times to add stories for multiple pages.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Once your added a few stories you can start Storybook locally with:

```bash
npm run storybook
```

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

You can see the changes you make in you code in realtime in your Storybook. &#x20;

The idea of Storybook is to easyly let you see the pages in different configuration without having to reproduce the full login/register cinematic in a real Keycloak.  \
Keycloakify provide a default mock context for every pages, the story let you partially overrride some specific part of this default mock to reflect pages in different configurations.  \
\
For example, if you want to create a story that show the register page in chinese you would add this:

{% code title="src/login/pages/Register.stories.tsx" %}
```tsx
import type { Meta, StoryObj } from "@storybook/react";
import { createKcPageStory } from "../KcPageStory";

const { KcPageStory } = createKcPageStory({ pageId: "register.ftl" });

const meta = {
    title: "login/register.ftl",
    component: KcPageStory
} satisfies Meta<typeof KcPageStory>;

export default meta;

type Story = StoryObj<typeof meta>;

export const Default: Story = {
    render: () => <KcPageStory />
};

export const InChinese: Story = {
    render: ()=> (
        <KcPageStory
            kcContext={{
                locale: {
                    currentLanguageTag: "zh-CN"
                }
            }}
        />
    )
};
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

That's really nice, however this approach has it's limits. At some point you'll want to test in a real Keycloak to make sure everything works. Also you don't nessesary know the kcContext values to provides to test replicate a desired configuration. &#x20;

Don't worry! Keycloakify got you covered by letting you test in a local Keycloak Docker container.

{% content-ref url="in-a-keycloak-docker-container.md" %}
[in-a-keycloak-docker-container.md](in-a-keycloak-docker-container.md)
{% endcontent-ref %}
