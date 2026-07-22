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

Single web app: **"Campanha do Povo que Batalha"**, a Portuguese-language campaign CRM. Stack: TanStack Start (SSR) + Vite + React 19 + Tailwind v4, with a **hosted (remote) Supabase** backend. **Bun** is the package manager (`bun.lock`, `bunfig.toml`); the `bun` binary lives at `~/.bun/bin` (added to PATH by its installer).

Standard commands live in `package.json` scripts — run them with Bun:
- Dev server: `bun run dev` → serves on `http://localhost:8080` (fixed/strict port, set by `@lovable.dev/vite-tanstack-config`).
- Lint: `bun run lint` (ESLint). Formatting is enforced via `prettier/prettier` ESLint rules; `bun run format` applies fixes.
- Build: `bun run build`.

Non-obvious caveats:
- The repo currently has **many pre-existing `prettier/prettier` lint errors** across `src/`. `bun run lint` exits non-zero on a clean checkout; treat those as baseline noise, not regressions from your change.
- `.env` only ships the **client** Supabase keys (`VITE_SUPABASE_*` publishable key). The client (landing page, login, password reset) works with just these.
- **Server-side** flows require `SUPABASE_SERVICE_ROLE_KEY`, which is NOT in `.env`. Any server route/function that uses `supabaseAdmin` (`src/integrations/supabase/client.server.ts`) — e.g. the public forms under `/api/public/*` (`/recadastro`, `/inscrever`, `/f/$slug`) and the authenticated CRM's server functions — throws `Missing Supabase environment variable(s): SUPABASE_SERVICE_ROLE_KEY`. Provide it as a Cursor secret to exercise those flows end to end.
- **Login is invite-only** (no public signup). Reaching the authenticated dashboard requires an existing account (test credentials). The public capture forms are the "front door" for a no-login supporter-capture action, but they still need the service role key server-side.
