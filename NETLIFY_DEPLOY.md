# Deploy RentFlow on Netlify

The project is ready: `netlify.toml` (build + security headers), `netlify/functions/agent-cron.mjs` (runs the AI agent every morning at 08:00 India time), `.gitignore`.

> Netlify's drag-and-drop upload does NOT work for this app (it needs a server build). Use GitHub (easiest) or the CLI.

## Option A: GitHub, then Netlify (recommended)
1. github.com → New repository (private) → "uploading an existing file". Unzip the project and drag in everything **inside** the `v0-rent-flow-app-main` folder (not the folder itself). Commit.
2. app.netlify.com → **Add new project → Import an existing project → GitHub** → pick the repo.
3. Build settings are read from `netlify.toml`. Leave them as they are. Do not click Deploy yet: first add the variables below (Project configuration → Environment variables), then deploy.

## Option B: CLI (no GitHub)
```
npm i -g netlify-cli
netlify login
netlify init        # create & link a new site
netlify env:set NEXT_PUBLIC_SUPABASE_URL "https://xxxx.supabase.co"   # repeat for each variable below
netlify deploy --build --prod
```

## Environment variables
| Name | Value |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase Project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon public key |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service_role key. Secret: mark it "Contains secret values" |
| `CRON_SECRET` | any long random string (mark secret) |
| `ANTHROPIC_API_KEY` | optional (mark secret) |
| `AGENT_MODEL` | optional, default `claude-haiku-5-5` |

Supabase setup (SQL schema, Google login, redirect URL) is in `SETUP_SUPABASE_AGENT.md`. In Supabase → Authentication → URL Configuration, set **Site URL** to your Netlify address (for example `https://yourname.netlify.app`).

## After the first deploy
1. Open the site on your phone → browser menu → **Add to Home Screen** (iPhone: Share → Add to Home Screen).
2. More → App & Help → Cloud → create your cloud account → Sync now → More → AI Agent → Check now.
3. Netlify → **Logs → Functions → agent-cron** shows each morning run. Scheduled functions only run on the published (production) deploy.

## If something fails
- *Build fails with "Secrets scanning found secrets"*: confirm `SECRETS_SCAN_OMIT_KEYS` in `netlify.toml` is unchanged and that `SUPABASE_SERVICE_ROLE_KEY` is never named `NEXT_PUBLIC_...`.
- *"Supabase is not configured on the server"*: a variable is missing or you did not redeploy after adding it.
- *Agent never runs by itself*: check `CRON_SECRET` exists and open Logs → Functions → agent-cron.
