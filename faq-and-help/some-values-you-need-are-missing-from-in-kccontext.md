---
description: Extending the KcContext type definition
---

# 😖 Some values you need are missing from in kcContext type definitions?

The kcContext type definitions only includes what the default pages actually uses. &#x20;

However, at runtime there is much more information in the kcContext that what TypeScript is aware of! &#x20;

A common scenario is when you want, on the register page to enable sign-up via Google or Facebook but the kcContext.social object isn't defined:  \


<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p>kcContext.social not defined in the KcContext type on the register page.</p></figcaption></figure>

Here is how you would go about addressing this usecase for this particular usecase: &#x20;

First step is to extend the KcContext type:

<figure><img src="https://github.com/user-attachments/assets/012aa574-868c-4f7b-a13e-220f1f12ce2a" alt=""><figcaption><p>Declaring that there is a social property on the kcContext of the register page and this social object is of the same type as the one present on the login page.</p></figcaption></figure>

&#x20;Secont step is to augment the KcContext base mocks for testing in Storybook: &#x20;

<figure><img src="https://github.com/user-attachments/assets/fcb4ee74-6043-4fe4-833e-96cb0b86ef51" alt=""><figcaption><p>We say that the social object of the mock of the register page kcContext is the same as the social object on the mock of the login page kcContext</p></figcaption></figure>

Then you can use the kcContext.social object in the register page just as you would on the login page:

![Applying the same socialProviderNode on the register page as the one applied in the login page.](https://github.com/user-attachments/assets/c142cae5-05d0-45d5-97ff-b93dee4fc2d6)

