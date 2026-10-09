# sveltekit-base-root-trailing-slash

Reproduction for the base root ignoring `trailingSlash: "never"`.

This is a fresh `sv create` minimal app with three changes:

- `vite.config.ts` sets `paths: { base: '/app' }` and uses `adapter-node`
- `src/routes/+layout.ts` exports `trailingSlash = 'never'`
- `src/routes/about/+page.svelte` exists as a non-root route to compare against

## Steps

```sh
pnpm install
pnpm build
node build
```

In another terminal:

```sh
curl -sI http://localhost:3000/app
curl -sI http://localhost:3000/app/about/
```

## Expected

`/app` returns 200, and `/app/` redirects to `/app`, the same way `/app/about/` redirects to `/app/about`.

## Actual

`/app` returns a 308 to `./app/`. The root is the only route that ignores `trailingSlash`.
