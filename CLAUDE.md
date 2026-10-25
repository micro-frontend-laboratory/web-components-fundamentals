# CLAUDE.md

This repository, `web-components-fundamentals`
(https://github.com/micro-frontend-laboratory/web-components-fundamentals), belongs to the GitHub
organization `micro-frontend-laboratory` (https://github.com/micro-frontend-laboratory), where it
is part 1 of 5. It shows custom elements written with nothing but the browser:
`apps/basics/web-components-basics`, three components in plain JavaScript.
The audience reads the code on screen, so the code must be clean and obvious.

README and `docs/` are for someone who downloads the repo: what the app is, how to run it, how
it works. Build warnings and workarounds go in the pitfalls below, not there.

## Rules

1. **Never change the contract.** Tags (`user-card`, `counter-button`, `todo-list`), their
   attributes, the `addItem` method, the events `count-changed` and `item-added` with their
   `detail`, and port 4100.
2. **Never commit to `main`.** Each change is built on its own branch `feature/<what we built>`.
   When it is done, stop and let the user review it. Commit and push only when the user says so.
   Pull requests are opened from the feature branch into `main`.
3. **No comments in code.** Names carry the meaning. Explanations belong in `docs/`.
4. **No test suites.** Verify each change yourself (lint, open it in a browser), but do not commit
   test files.
5. **Show data with `textContent`**, never `innerHTML`.
6. **Plain JavaScript, no build.** The browser runs the files as they are. Keep dependencies few
   and well known; the only ones are ESLint's, for `npm run lint`.
   **The user runs every `npm` and `npx` command.** Give the exact command and folder, then wait.
   Write `package.json` first and let the user review every dependency before installing.
7. **No Prettier.** Formatting is VS Code's built-in formatters, set in the committed
   `.vscode/settings.json`, plus `.editorconfig`: 2-space indent, single quotes, semicolons,
   lines under ~100 chars.
8. **Everything runs through Docker.** The page is served by `nginx:stable-alpine` with no npm
   at runtime. Use Compose Watch (`develop.watch`), not bind mounts. No production images.

## Layout

| Folder | What |
| --- | --- |
| `apps/basics/web-components-basics` | The three components, the demo page, `nginx.conf`, `Dockerfile.dev` |
| `compose.basics.yaml` | The `shop-basics` group, published on `localhost:4100` |
| `tools/scripts/basics` | `deploy-web-components-basics.bat` |
| `docs/guides` | How the components are built |

```bash
docker compose -f compose.basics.yaml up --build --detach --wait
docker compose -f compose.basics.yaml watch
```

## Known pitfalls

| Symptom | Fix |
| --- | --- |
| nginx container is "unhealthy" but the page loads | Alpine resolves `localhost` to `::1`: nginx needs `listen [::]:80;` as well as `listen 80;` |
| Edits do not reach the container | Start with `--watch`, and check the service's `develop.watch` paths |
| An event never arrives | Event names are case-sensitive, and the listener must be on the element itself |
| `title` shows as a tooltip over the whole component | `title` is a global HTML attribute; `todo-list` uses `heading` |
