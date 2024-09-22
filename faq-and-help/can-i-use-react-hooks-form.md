# 🤓 Can I use react-hooks-form?

You can but it's probably not a good idea.\
The Keycloak server is authoritative for defining if a given input is valid, not your theme.\
For example, it's on the Keycloak server that you would defines that user passwords must be at least 12 character long or that you only accept registration from emails _@your-company.com_.\
You don't want to hard code those rules in your theme just to be able to display errors in realtime as the user fills the form, it will be a nightmare to maintain because you'll have to keep your theme in sync with your Keycloak server configuration.

\
The validation criteria are made accessible to the clients in `kcContext.profile.attributesByName[*].validators` and `kcContext.passwordPolicies`.\
Implementing those validator manually within `react-hooks-form` isn't straight forward. There's a lot of cases and the format is specific to Keycloak.

\
What you want to do instead is use the [useUserProfileForm hook](https://github.com/keycloakify/keycloakify/blob/8eaaffb25a7b6d6c8b7e455d5005dc31d70b8927/src/login/UserProfileFormFields.tsx#L20-L27) provided by Keycloakify which is akin to the react-hooks-form API wise but already implements all the Keycloak validators and password policies.

Beyond that don't feel the need to implement every possible input fields like checkboxes, multi-value, etc. You can just implement the subset fields type your client or company actually uses.
