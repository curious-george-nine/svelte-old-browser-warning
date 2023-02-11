# svelte-old-browser-warning

## how to use

First, create your project.

```bash
npm create svelte
cd my-project/
npm i
```

then, install svelte-old-browser-warning.

```bash
npm i svelte-old-browser-warning
```

then use it in +layout.svelte:

```svelte
<script>
  import { BrowserWarningAlert } from "svelte-old-browser-warning";
</script>

<BrowserWarningAlert>
```
