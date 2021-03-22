# snowpack-plugin-watchappcss

This simple plugin is a temporary fix until https://github.com/snowpackjs/snowpack/issues/2916 is solved

## How to use

```
npm install --save-dev @rohfle/snowpack-plugin-watchappcss
```

then add `@rohfle/snowpack-plugin-watchappcss` to your plugins in `snowpack.config.js`

## Issue background

Files defined by the purge rule in `tailwind.config.js` are not being watched for changes, so any svelte changes will not trigger reprocessing of App.css.

## Details

- `@snowpack/plugin-postcss` transforms App.css (which contains `@tailwind base` etc). Postcss starts with the full tailwind styles, then shakes the tree to create a reduced css file. This reduced file has only the classes found in files defined by the purge rule above.
- When a svelte file change is detected, @snowpack/plugin-svelte processes the file. If the file contains a `<style>` tag, it will be processed by svelte-preprocessor using the postcss plugin.
- Because the class attributes in the svelte file are only used during the purge in App.css, and there is no file change in App.css, the new classes will not be added
