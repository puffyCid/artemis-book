# Limitations

There are currently some limitations to scripting with `artemis` and Deno.

1. All scripts executed through `artemis` must by synchronous. Async scripts are
   **not** supported.
2. All scripts executed through `artemis` must be in `JavaScript`. You
   **cannot** execute `TypeScript` scripts directly. You **must** compile and
   bundle them into one (1) `JavaScript` file.
3. The `JavaScript` must be in common JS format (cjs). EMCAScript (ES) module
   scripts are not supported. The example code below uses `esbuild` to bundle
   the `main.ts` file to `JavaScript` using CJS format via `deno run build.ts`:

```typescript
import * as esbuild from "https://deno.land/x/esbuild@v0.15.10/mod.js";
import { denoPlugin } from "https://deno.land/x/esbuild_deno_loader@0.6.0/mod.ts";

async function main() {
  const _result = await esbuild.build({
    plugins: [denoPlugin()],
    entryPoints: ["./main.ts"],
    outfile: "main.js",
    bundle: true,
    format: "cjs",
  });

  esbuild.stop();
}

main();
```

Adding support for some of these features should be possible and may be added in
a future release of `artemis`.
