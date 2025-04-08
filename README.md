# deno-vite-ssr-playground

Deno and vite playground for SSR.

## Commands run to create this app

From <https://vite.dev/guide/ssr.html>

```sh
npm create vite@latest
    > Others
    > Extra Vite Starters
    > ssr-vanilla
```

Note that there is a Deno Vite plugin:
<https://github.com/honojs/vite-plugins>

It doesn't seem have Deno as a build adpater listed there:
<https://github.com/honojs/vite-plugins/tree/main/packages/build>

But do have a deno file there:
<https://github.com/honojs/vite-plugins/blob/main/packages/build/src/adapter/deno/index.ts>

## Tranformation made

- Back up original Javascript file from `server.js` to `server-backup.ts`
- Duplicated `server.js` to `server.ts` and fixed TS types
  - Remove compression middleware (TS issues)
  - Change NodeJS `process.env` to `Deno.env.get()`

## Commands

```sh
npm install # or deno install
deno run -A server.ts
```

## Known issues

### Missing symbol

Run `deno run -A server.ts` error:

```sh
dyld[73020]: missing symbol called
zsh: abort      deno run -A server.ts
```

It seems to be failing on this line specifically:

```ts
const { createServer } = await import('vite');
```

An attempt to add [@deno/vite-plugin](https://github.com/denoland/deno-vite-plugin) on [this branch](https://github.com/soundstep/deno-vite-ssr-playground/compare/main...deno-vite-plugin) has been made but the command (originally `node server.js`) from the vite template is not using the vite binary directly, and probably not the vite config either.

Reported to the Deno team:
<https://discord.com/channels/684898665143206084/1359100251784614080>
