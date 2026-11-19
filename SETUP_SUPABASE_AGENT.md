# RentFlow: cloud account + AI agent setup (about 20 minutes)

The AI agent runs on the server every morning (and when you press **Check now**). It reads your synced data, prepares work, and shows it in **More → AI Agent**. You review and tap.

## 1. Create the Supabase project
1. supabase.com → New project (free plan is fine). Choose a region near you (Mumbai).
2. **SQL Editor** → paste everything from `supabase/schema.sql` → Run.
3. **Project Settings → API**: copy the *Project URL*, the *anon public* key and the *service_role* key.
4. **Authentication → Providers**: Email is on by default. For "Continue with Google", enable Google and paste your Google OAuth client ID/secret (optional). Add your Vercel URL under **Authentication → URL Configuration → Site URL**.
5. Optional: Authentication → Providers → Email → switch off "Confirm email" if you want to skip the confirmation mail.

## 2. Add environment variables (Netlify or Vercel)
Project → Settings → Environment Variables (all environments), then **Redeploy**:

| Name | Value |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | anon public key |
| `SUPABASE_SERVICE_ROLE_KEY` | service_role key. **Secret.** Never add NEXT_PUBLIC_ to it |
| `CRON_SECRET` | any long random string (Vercel sends it to the cron job automatically) |
| `ANTHROPIC_API_KEY` | optional: daily summary + understanding messy repair text |
| `AGENT_MODEL` | optional, default `claude-haiku-5-5` |

Scheduling: on Vercel, `vercel.json` runs the agent daily at 08:00 India time. On Netlify, `netlify/functions/agent-cron.mjs` does it (see NETLIFY_DEPLOY.md).

## 3. First use
1. Open the app → **More → App & Help → Cloud** → *Create account* (or Sign in). The same account on every device that manages this property.
2. Press **Sync now**. Your data is copied to the cloud (it merges, nothing is overwritten blindly).
3. **More → AI Agent → Check now**. After that it runs by itself each morning.
4. To publish your late-fee rules and UPI ID to the agent: **More → Fees & Receipt → Save** once.

## What the agent does
| Job | Does it by itself? |
|---|---|
| Rent reminders (3 days before, due day, +1, +3, +7, +15, then weekly) with your late fee and UPI ID | Prepares the message. **You tap Send on WhatsApp** |
| Tenant says "I paid" | Puts it in your queue. **You confirm** after checking bank/UPI. Never automatic |
| Next month's bills | **Yes**, on the day you choose (default 25th). Meter readings are left for you |
| Missing bills this month | Asks you |
| New repair request | **Yes**: sets urgency and type. Assigning a mistri needs your tap |
| Stuck repairs, agreements ending, duplicate payments | Flags them |
| Morning summary | Written by Claude if you added the key (numbers are checked against real data) |

## Smarter AI (needs `ANTHROPIC_API_KEY`)
- **Per-tenant reminders:** the agent studies how each tenant pays (on-time record, average days late, trend, broken promises) and Claude writes the reminder in a matching tone. The reason is shown on every card ("Why: ..."). The exact amount is verified; if the AI text is wrong the plain template is used.
- **Ask the agent:** type a question in More → AI Agent ("Who hasn't paid this month?", "What did I spend on repairs this year?"). Claude can only call read-only tools on your data. Limit: 40 AI questions/messages per day.
- **Paste a tenant's reply:** paste a WhatsApp message in any language. It detects a promise date, "I already paid" (with UTR), or a dispute, and offers one-tap buttons. Claims still wait for your confirmation.
- **Patterns:** tenants paying later each month, electricity spikes, rooms with repeated repairs, unusually expensive repairs.
- **Repairs:** a polite message to the mistri is drafted for you.
- Cost: with the default small model, roughly a few rupees per day for a small property. Reminder drafts are capped at 8 per run.

## Safety design
- The agent never changes amounts, never confirms money, never sends messages.
- Claude only sees the minimum text, treats tenant text as untrusted, and its output is validated. If anything fails, the plain rules still work.
- All data goes through server code with your login. The database has no public access (row level security on, no public policies).
- Every approval is written to the Activity log.

## Known limits
- **Messages are not sent automatically.** WhatsApp links need a human tap. Fully automatic sending needs the WhatsApp Business API (business verification + approved templates). That is the next step if you want it.
- Tenants still cannot log in on their own phones. That needs per-tenant accounts and access rules (next phase).
- Approved actions run on the device where you tap them. If a record is not on that device yet, press Sync now and retry.
- Not tested against a live Supabase project, a real Google/OAuth round trip, or the live Claude API from this build environment. The agent logic, API wiring and inbox were tested with local stand-ins.
