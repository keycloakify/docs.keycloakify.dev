# ⬆️ CRA -> Vite

Starting with Keycloakify 9.4 Vite is now supported by Keycloakify! 🥳

That being said it will remain compatible with WebPack for the forseeable future.

If you wish to upgrade your theme that was based on [the CRA starter project](https://github.com/keycloakify/keycloakify-starter-cra) here is what you need to do:

* Clone the new starter and copy/paste your `src` and `public` directories as well as your `index.html` into it.
* Rename the src/index.tsx into src/main.tsx
* Check if you have modified other parts since you cloned the original starter
* The build directory is now dist and the build\_keycloak directory is now dist\_keycloak update your .github/workflow/ci.yaml file and your Dockerfile to reflect theses changes. If you haven't changed anything you can just keep things as they are in the new starter.
* If you are hosting you app on GitHub pages, don't forget to report your homepage field from your old package.json to the new one. You also want to specify a base option in the vite.config.ts if your app was hosted under a subpath which is the case with the default GitHub Page domains (`<username>.github.io/<project name>`)
* Of course report all the dependencies you might had added in the the new package.json
* Storybook has been upgraded to v8, have a look at the story in the new starters. They are slightly different.
* You can remove the `src/PUBLIC_URL.ts` file if you used one. You can now simply use `import.meta.env.BASE_URL` to reference assets in your public directory like: ``<img src={`${import.meta.env.BASE_URL}foo.png`} />``
* You now don't have to specify a XDG\_CACHE\_HOME in your CI. There is a improved builtin cache mechanism in v9.4 that dramatically improve build speed. [If you still provide the XDG\_CACHE\_HOME var env](https://github.com/keycloakify/keycloakify-starter-cra/blob/2da558a3e7c0e1a4c420eda14adb9ecdd4284ee8/.github/workflows/ci.yaml#L21) you must also provide it to the yarn build script.
* (OPTIONAL) You might want to move your Keycloakify configuration from your `package.json` to your `vite.config.ts`. You just need to move your keycloakify field in your package.json to the argument of the Keycloakify vite plugin. See [here](../../configuration-options/) for examples.

{% content-ref url="../../keycloakify-in-my-codebase/" %}
[keycloakify-in-my-codebase](../../keycloakify-in-my-codebase/)
{% endcontent-ref %}
