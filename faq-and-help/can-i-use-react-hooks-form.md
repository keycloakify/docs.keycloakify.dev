# 🤓 Can I use react-hooks-form?

You can but it's probably not a good idea.  \
The Keycloak server is authorithative for defining if a given input is valid, not your theme.  \
For example, it's on the Keycloak server that you would defines that user passwords must be at least 12 character long or that you only accept registration from emails _@your-company.com_.  \
You don't want to hard code thoses rules in your theme, it will be a nightmare to maintain because you'll have to keep your theme in sync with your Keycloak server configuration.   &#x20;

\
The validation criteria are made accessible to the cliens in `kcContext.profile.attributesByName[*].validators` and `kcContext.passwordPolicies`.  \
Implementing thoses validaror manually witin `react-hooks-form` isn't straight forward. There's a lot of cases and the format is specific to Keycloak. &#x20;

\
What you want to do instead is use the [useUserProfileForm hook](https://github.com/keycloakify/keycloakify/blob/8eaaffb25a7b6d6c8b7e455d5005dc31d70b8927/src/login/UserProfileFormFields.tsx#L20-L27) provided by Keycloakify which is akin to the react-hooks-form API wise but already implements all the Keycloak validators and password policies.&#x20;

Beyond that don't feel the need to implement every possible input fields like checkboxes, multivalued, ect. You can just implement the subset fields type your client or company actually uses.
