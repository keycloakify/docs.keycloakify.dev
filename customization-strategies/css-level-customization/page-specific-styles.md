# Page specific styles

So far the customization we have made applies to all the pages however you might want to have stylesheet specific to certain pages.

You can do that by loading different stylesheet and applying different classes depending on the `kcContext.pageId`.

Implementation example, instead of importing our stylesheet at the top of the KcPage.tsx component file we import them dynamically:

<pre class="language-tsx" data-title="src/login/KcPage.tsx"><code class="lang-tsx">import {
    Suspense, 
    lazy,
<strong>    useMemo
</strong>} from "react";

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    const { i18n } = useI18n({ kcContext });

<strong>    const classes = useCustomStyles(kcContext);
</strong>
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
                                doUseDefaultCss={true}
                                UserProfileFormFields={UserProfileFormFields}
                                doMakeUserConfirmPassword={doMakeUserConfirmPassword}
                            />
                        );
                }
            })()}
        &#x3C;/Suspense>
    );
}

<strong>function useCustomStyles(kcContext: KcContext) {
</strong><strong>    return useMemo(() => {
</strong><strong>        
</strong><strong>        // You stylesheet that applies to all pages.
</strong><strong>        import("./main.css");
</strong><strong>        let classes: { [key in ClassKey]?: string } = {
</strong><strong>            // Your classes that applies to all pages
</strong><strong>        };
</strong>
<strong>        switch (kcContext.pageId) {
</strong><strong>            case "login.ftl":
</strong><strong>                // You login page specific stylesheet.
</strong><strong>                import("./pages/login.css");
</strong><strong>                classes = {
</strong><strong>                    ...classes,
</strong><strong>                    // Your classes that applies only to the login page
</strong><strong>                };
</strong><strong>                break;
</strong><strong>            case "register.ftl":
</strong><strong>                // Your account page specific stylesheet
</strong><strong>                import("./pages/register.css");
</strong><strong>                classes = {
</strong><strong>                    ...classes,
</strong><strong>                    // Your classes that applies only to the register page
</strong><strong>                };
</strong><strong>                break;
</strong><strong>            // ...
</strong><strong>        }
</strong>
<strong>        return classes;
</strong>
<strong>    }, []);
</strong><strong>}
</strong></code></pre>

## What's next?

At this point of the documentation, if you're looking for using tailwind you can go to this page:

{% content-ref url="using-tailwind.md" %}
[using-tailwind.md](using-tailwind.md)
{% endcontent-ref %}

Else you can skip directly to the next section: &#x20;

{% content-ref url="using-custom-assets/" %}
[using-custom-assets](using-custom-assets/)
{% endcontent-ref %}
