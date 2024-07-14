# Page specific stlyes

So far the customisation we have made applies to all the pages however you might want to have stylesheet specific to certain pages. &#x20;

You can do that by loading diferent stylesheet and applying different classes depending on the `kcContext.pageId`.

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
</strong><strong>
</strong><strong>        switch (kcContext.pageId) {
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
</strong><strong>
</strong><strong>        return classes;
</strong><strong>
</strong><strong>    }, []);
</strong><strong>}
</strong></code></pre>

Now some specific documentation if you wish to use [Tailwind](https://tailwindcss.com/):

{% content-ref url="../../importing-assets/in-styles/tailwind.md" %}
[tailwind.md](../../importing-assets/in-styles/tailwind.md)
{% endcontent-ref %}
