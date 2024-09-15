# Using Tailwind

{% hint style="info" %}
Even if you're only interested by Tailwind you should still read the other section of the [CSS Level Customization](./) first as it gives important context.
{% endhint %}

To use Tailwind in your Keycloakify project start by following the setup guide for Vite.

{% embed url="https://tailwindcss.com/docs/guides/vite#react" %}

Beyond that, here is a demo setup of light modification of the starter template to incorporate tailwind:

{% embed url="https://github.com/keycloakify/keycloakify-starter/tree/tailwind" %}

{% embed url="https://private-user-images.githubusercontent.com/6702424/345903122-3f06a287-03e3-4441-a4e9-e4ea6b76f388.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjA5ODk4NDIsIm5iZiI6MTcyMDk4OTU0MiwicGF0aCI6Ii82NzAyNDI0LzM0NTkwMzEyMi0zZjA2YTI4Ny0wM2UzLTQ0NDEtYTRlOS1lNGVhNmI3NmYzODgucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDcxNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA3MTRUMjAzOTAyWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZGI5ZjU5MzkxY2MzYmQ3MTNkZWM5NWUwOWM3MDg1MGJlYjQ4ODdhZjVlMTRlOGZlMDllMmM3MzQwYTJmNTgyYyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.4EdrQ-RKBP3SDeZ865IYyO2wb2ctkHEkdV_JAqquFQI" %}
Preview on the 'tailwind' branch for the starter template&#x20;
{% endembed %}

What has been done:

* [Applying some custom tailwind utilities classes using the @apply directive](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L7-L14).
* Using the [Geist](https://vercel.com/font) font, [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/tailwind.config.js#L9-L11), [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L1) and [here](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/index.css#L9).
* Ejecting the [login.ftl](https://storybook.keycloakify.dev/?path=/story/login-login-ftl--default) page (`npx keycloakify eject-page` and _login -> login.ftl_) and [applying a tailwind class](https://github.com/keycloakify/keycloakify-starter/blob/dd516e53e4dfa7c1ce02bab557420b999e87eca2/src/login/pages/Login.tsx#L172).

Here is the summary of the changes:

{% embed url="https://github.com/keycloakify/keycloakify-starter/commit/e6c71f13acbc65ccb8f57172c45e8c04a2151007" %}
