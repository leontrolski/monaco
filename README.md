# Bundled Monaco editor

The [Monaco editor](https://microsoft.github.io/monaco-editor/) lists no steps for vendoring and using with plain ol' JS modules, and I can't easily find a CDN that provides a bundle.

This repo contains a bundled version of the js and css ready for use. Editor options are listed [here](https://microsoft.github.io/monaco-editor/docs.html#interfaces/editor.IEditorOptions.html). Either use the CDN as below or vendor the files.

Example usage:

```html
<style>#container{height:20rem;width:50rem;}</style>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/leontrolski/monaco@main/monaco.css">

<div id="container"></div>

<script type="module">
import {monaco} from "https://cdn.jsdelivr.net/gh/leontrolski/monaco@main/monaco.js";
// Just disable all the service workers
self.MonacoEnvironment = {getWorker: () => new Worker(URL.createObjectURL(new Blob([`onmessage = () => {}`])))}
// Start an editor
const editor = monaco.editor.create(document.getElementById("container"), {
    value: "def f():\n    return 42",
    language: "python",
    theme: 'vs-dark',
    lineNumbers: 'off',
    automaticLayout: true,
    minimap: {enabled: false},
})
// Do something on change
editor.onDidChangeModelContent(() => console.log(editor.getValue()))
</script>
```

## Steps to reproduce bundles

```shell
git clone git@github.com:leontrolski/monaco.git
cd monaco
npm install  # this was run with npm@10.5.2 node@v20.13.1
npm build
```

Maybe one day I'll set up some CI to keep it up to date...
