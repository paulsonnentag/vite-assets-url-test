# Importing assets as URL doesn't work if other module exports a variable named URL

Build the project and server the dist folder with a static file server

```
yarn build
cd dist
npx http-server
```

Open it in the browser and you will get the following error:

```
index-8Zvfj42v.js:35 Uncaught TypeError: yt is not a constructor
    at index-8Zvfj42v.js:35:364
```

If you comment out the base property in the vite config and rebuild the project everything works

```
// https://vitejs.dev/config/
export default defineConfig({
  //base: "./",
  plugins: [react()],
});

```

My suspicion is that the shadowing of URL is caused by `react-dnd` which `react-arborist` depends on. The following [file](https://github.com/react-dnd/react-dnd/blob/7c88c37489a53b5ac98699c46a506a8e085f1c03/packages/backend-html5/src/NativeTypes.ts#L3) exports a property named URL:

```
export const FILE = '__NATIVE_FILE__'
export const URL = '__NATIVE_URL__'
export const TEXT = '__NATIVE_TEXT__'
export const HTML = '__NATIVE_HTML__'
```
