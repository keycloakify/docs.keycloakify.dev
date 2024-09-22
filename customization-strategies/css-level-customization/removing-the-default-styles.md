# Removing the default styles

## Case by case

You may notice that beside the the kcSomething classes other classes are applied to the components.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption><p>The &#x3C;header> element has an extra class beside kcFormHeaderClass: login-pf-header</p></figcaption></figure>

This other classes, non prefixed with kc actually have styles rules that target them. Let's as an example remove the login-pf-header class:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import "./main.css";
// ...

export default function KcPage(props: { kcContext: KcContext }) {
  // ...
}

const classes = {
<strong>    kcFormHeaderClass: ""
</strong>} satisfies { [key in ClassKey]?: string };
</code></pre>

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>We can see that now, the &#x3C;header> element only have the kcFormHeaderClass, the login-pf-header class has been removed. As a result the text is now aligned to the left (default layout)</p></figcaption></figure>

On some components, multiples utility classes are applied, you may want to keep some of them and remove others. Example:

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Here we can see that a bunch of col-* classes gets applied to the element with kcInputWrapperClass</p></figcaption></figure>

Let's say, for example, that we would like to remove keep only the col-md and lg classes. To do that that we would write:

{% code title="src/login/KcPage.tsx" %}
```tsx
import "./main.css";
// ...

export default function KcPage(props: { kcContext: KcContext }) {
  // ...
}

const classes = {
  kcInputWrapperClass: "col-md-12 col-lg-12",
} satisfies { [key in ClassKey]?: string };
```
{% endcode %}

Result:

<figure><img src="../../.gitbook/assets/image (17).png" alt="" width="375"><figcaption></figcaption></figure>

## Removing all the default styles at once

Maybe you'd prefer to remove all default styles at once you can do that by setting doUseDefaultCss to false.

<pre class="language-tsx" data-title="src/login/KcPages.tsx"><code class="lang-tsx">export default function KcPage(props: { kcContext: KcContext }) {

    return (
        &#x3C;Suspense>
            {(() => {
                switch (kcContext.pageId) {
                    default:
                        return (
                            &#x3C;DefaultPage
                                kcContext={kcContext}
                                i18n={i18n}
                                classes={classes}
                                Template={Template}
<strong>                                doUseDefaultCss={false}
</strong>                                UserProfileFormFields={UserProfileFormFields}
                                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
                            />
                        );
                }
            })()}
        &#x3C;/Suspense>
    );
}
</code></pre>

However be aware that re-styling everything involves quite a bit of work:

<figure><img src="../../.gitbook/assets/image (18).png" alt="" width="375"><figcaption><p>The login page with doUseDefaultCss set to false</p></figcaption></figure>

Up next:

{% content-ref url="applying-your-own-classes.md" %}
[applying-your-own-classes.md](applying-your-own-classes.md)
{% endcontent-ref %}
