<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

## Cursor Cloud specific instructions

- Package manager is **Bun** (`bun.lock`); `bun` is installed and symlinked into `/usr/local/bin`. The startup update script runs `bun install`. Scripts live in `package.json`: `bun run dev` (Vite dev server on port `8080`), `bun run build`, `bun run lint`, `bun run format`. There is no test framework/script in this repo.
- `bun run lint` currently reports thousands of pre-existing `prettier/prettier` violations in committed code — this is the repo's state, not an environment problem. Do not mass-reformat unrelated files.
- Supabase is a **remote hosted** project (no local Supabase). The committed `.env` only has the public `SUPABASE_URL`/`SUPABASE_PUBLISHABLE_KEY`, so client-side auth works, but any server route/function using the admin client (most `/api/public/*` form endpoints, `bootstrap-admin`, admin ops) throws without `SUPABASE_SERVICE_ROLE_KEY`. Set that env var to exercise server-side DB and public-form flows.
- Login (`/auth`) is invite-only (checks `user_roles`), so reaching the authenticated CRM needs an existing Supabase account. CEP lookup (`/api/public/cep/:cep`) and geocoding hit public external APIs and need no secret.
- `vite.config.ts` intentionally minimal: do NOT re-add plugins already bundled by `@lovable.dev/vite-tanstack-config` (tanstackStart, react, tailwind, nitro, etc.) or the app breaks with duplicate plugins.
