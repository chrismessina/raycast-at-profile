# Contributing to @ Profile

Thanks for helping. This guide covers running @ Profile locally and sending a change.

## Where changes go

@ Profile is published in the [Raycast Store](https://www.raycast.com/chrismessina/at-profile).
Open pull requests against [`raycast/extensions`](https://github.com/raycast/extensions/tree/main/extensions/at-profile),
not this repository: Raycast reviews every change there, and merged changes come back here in a
sync pull request. Open [issues](https://github.com/chrismessina/raycast-at-profile/issues) here or in
`raycast/extensions`, whichever you prefer.

## Run it locally

You need [Raycast](https://www.raycast.com/) and Node.js 22.22.2 or later.

1. In Raycast, find @ Profile in the Store and choose **Fork Extension** from the action panel.
   Raycast checks out only this extension's folder from `raycast/extensions`.
2. In that folder, run `npm ci`, then `npm run dev`. Raycast opens the development version, and
   saving a file reloads it.

## Scripts

| Script | What it does |
| --- | --- |
| `npm run dev` | Run the extension in Raycast and reload on save |
| `npm run build` | Build the distribution bundle into `dist/` |
| `npm run lint` | Run Raycast's ESLint and Prettier checks |
| `npm run fix-lint` | Fix what the linter can fix |
| `npm test` | Run the unit tests (Vitest, in `test/`) |
| `npm run evals` | Run the AI evals in `ai.yaml` |
| `npm run sync-catalog` | Refresh `src/catalog/sherlock.json` from Sherlock's site list, probing each site, then the favicons |
| `npm run sync-favicons` | Refresh the bundled favicons without probing the catalog |
| `npm run publish` | Open or update the pull request in `raycast/extensions` (`ray publish`) |
| `prepublishOnly` | Stops `npm publish`: the extension goes to the Raycast Store, never to the npm registry |

Run `sync-catalog` from a normal connection. Under a sandbox or a throttled network most sites come
back unverified. `CONCURRENCY` sets how many sites it probes at once (default 8).

## How it works

- **Apps.** App data is managed through `src/helpers/apps.ts`. Custom apps are stored in Raycast's
  local storage (`src/helpers/storage.ts`), and app settings (show/hide) are persisted separately.
- **Built-in apps.** To add a default app, edit `src/utils/default-apps.ts`:

  ```typescript
  export const defaultApps: App[] = [
    // ... existing apps
    {
      name: "New App",
      value: "newapp",
      urlTemplate: "https://newapp.com/{profile}",
      placeholder: "username",
    },
  ];
  ```

  `{profile}` in `urlTemplate` is replaced with the handle. The `App` type is in `src/types/index.ts`.
- **Catalog.** `src/catalog/` holds the site catalog from the
  [Sherlock project](https://github.com/sherlock-project/sherlock) (`sherlock.json`, licensed in
  `LICENSE-sherlock`), the platform checks (`platforms.ts`), the sites Find Profile checks by default
  (`defaults.ts`), and the sync logic `npm run sync-catalog` uses (`sync.ts`).
- **Commands and tools.** Each command is a file at the top of `src/`, with views in
  `src/components/` and forms in `src/forms/`. The AI tools (`open`, `history`, `find-profiles`)
  are in `src/tools/`, and their instructions and evals in `ai.yaml`.

## Before you open a pull request

- `npm run build`, `npx tsc --noEmit` and `npm run lint` all pass, and so does `npm test`.
- You tried the change in Raycast, including its error and empty states.
- `CHANGELOG.md` has a new entry at the top, dated `{PR_MERGE_DATE}`. Raycast fills in the date
  when the pull request merges.
- New or changed screenshots go in `metadata/`, taken with Raycast's Window Capture.
