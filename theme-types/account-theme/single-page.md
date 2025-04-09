---
description: Customizing the Single Page Account UI
---

# Single-Page

To initialize a Single-Page account theme run the following command:

```bash
npx keycloakify initialize-account-theme # Select 'Sigle-Page'
```

{% embed url="https://youtu.be/UKU6zGCH-CY" %}
Video Tutorial
{% endembed %}

📌 Timestamps:\
[00:00](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=0s) – Intro\
[03:33](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=213s) – Changing the logo\
[07:46](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=466s) – Using a custom button component\
[14:13](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=853s) – Update process\
[16:55](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=1015s) – Translations (i18n)\
[18:23](https://www.youtube.com/watch?v=UKU6zGCH-CY\&t=1103s) – Enabling your account theme in the Keycloak Admin Console

## Initialize the Account 

After initialization, there will be many files that are part of the theme but are currently git-ignored. To customize a file and ensure it's tracked, run the own command included at the top of the file. For instance, at the top of `KcPage.tsx` you'll see `npx keycloakify own --path "account/KcPage.tsx"`.

When a new person clones the project and runs the installation script, their project will populate with all the necessary files while continuing to be git-ignored.

## Running the Server

To preview the application use `npx keycloakify start-keycloak`. This will run your application within a Keycloak container. Open the local realm version and select the Account theme link. Once that is open, you'll be able to work like you normally would on any React application with instant refresh and reload. 

**Note** 

If you have a Login theme where you are forcing a specific template to render, make sure to comment that out. Otherwise, you will never be able to log in.

## Updating the Logo

1. Own the `Header.tsx` (`src/account/root/Header.tsx`) file.
2. Add your own logo to the `Assets` (`src/account/assets`) folder.
3. Update the logo import to your new logo. `import logoSvgUrl from "../assets/logo.svg";` becomes `import logoSvgUrl from "../assets/your-logo.svg";` or whatever.
4. Run the server with `npx keycloakify start-keycloak`. 

## Updating a Custom Component

Updating a component that comes from PatternFly, means that we need to create a new component in some fashion. In order to make it possible to edit the components that are used from PatternFly, those are re-exported from the core library of PatternFly. 

1. In the project structure, locate `/src/shared/@patternfly/.../index.tsx` of whatever component or item you want to edit. Run the own command for that specific file. 
2. For the sake of this example, let's create a "CustomButton" component from the `@patternfly/react-core` package. Create a new file `CustomButton.tsx` at the same level as the index file (e.g., `/src/shared/@patternfly/react-core/CustomButton.tsx`).
3. Inside that component, you can create it however you want. For example, you could do

```tsx
import { type ButtonProps, Button } from "@patternfly/react-core";
import { css } from "@patternfly/react-styles";
import styles from "./CustomButton.module.css"; // Import the CSS module

export function CustomButton(props: ButtonProps) {
  const { children, variant = "primary", className: className_props, ...rest } = props;

  const className = css(styles.button, styles[variant], className_props);

  if (variant === "link") {
    return (
      <Button {...rest} className={className} variant="link">
        {children}
      </Button>
    );
  }

  return (
    <button {...rest} className={className}>
      {children}
    </button>
  );
}
```

4. You'll notice that it imports a CSS module file. Create `CustomButton.module.css` right next to it and style however you want.
5. In the `index.tsx` file then export the component. This will then override the button exported by Patternfly and apply it to every page that imports that button. 

```tsx
export * from "@patternfly/react-core";
export { CustomButton as Button } from "./CustomButton";
```

## Update Instances

The core module for the account is `@keycloakify/keycloak-account-ui`. You can always update the minor versions without issues (simply update them and run the installation script).


## Translations

Each file has instructions at the top of it. Generally, just create an override file and not own the base translation file. This will help to make sure only those changes take effect. 

Read more in the [email theme](../email-theme.md). 
