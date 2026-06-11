---
name: launch-checklist
description: Use when the user's product is built (or nearly built) and they want to get it live and accessible to customers. Triggers on phrases like "create my launch checklist", "how do I deploy this", "help me launch", "get this live", "put this in production", "ship it to customers", "what do I need to do to go live", or any request for a step-by-step path from working code to a product customers can use. Audits the current codebase — stack, services, environment variables, payments, deploy config — then writes a plain-English, step-by-step launch guide to `docs/launch-checklist.md`. Every step is marked as something the user must do themselves, something their coding agent can do, or both together, and all technical terms are explained for non-technical founders.
license: MIT
metadata:
  author: BuilderOS
  version: "1.0"
---

# Launch Checklist

This skill turns "the app works on my machine" into "customers can use it." It reviews the actual codebase, works out everything standing between the current state and a live product, and writes a personalised, step-by-step guide the founder can follow without a technical background.

The voice is a patient senior engineer onboarding a smart non-technical founder: plain language, no unexplained jargon, never condescending. The founder is capable — they just haven't done this before.

## Step 1: Audit the codebase

Read the repository thoroughly before writing anything. Establish:

- **What the product is and where it runs** — web app, mobile app (App Store / Google Play), desktop, or a mix. Detect the framework, backend, database, auth, and payment providers from the code and dependencies — don't ask the user things the code answers.
- **Configuration state** — environment variables used in code vs. what's documented (`.env.example` or similar), hardcoded values that must become secrets, test/sandbox keys that need live equivalents.
- **Third-party services** — every external service the app calls, and what each needs in production (live API keys, webhook endpoints, billing enabled).
- **Deploy readiness** — existing deploy config (e.g. `vercel.json`, `Dockerfile`, `eas.json`), build scripts, whether the production build currently succeeds, test status.
- **Launch gaps** — things customers-facing products need that the code may lack: error tracking, analytics, legal pages (privacy policy, terms), a custom domain, database backups, app store assets for mobile.

Confirm the picture with the user in one short message (product type, stack, the services found) plus any genuinely unanswerable questions — e.g. do they own a domain, do they have accounts with the detected services, is there a launch deadline.

## Step 2: Write `docs/launch-checklist.md`

Write the guide to `docs/launch-checklist.md` (create the folder if needed). Structure:

**Header** — product name, detected stack in one plain-English sentence, estimated total time, and a legend:

- 🧑 **You** — needs your identity, accounts, payment details, or a decision. An agent can't (or shouldn't) do this for you.
- 🤖 **Agent** — paste the given prompt into your coding agent and it can do this in the codebase or via the command line.
- 🤝 **Together** — the agent prepares it, you click the final button or paste in a value.

**Phases, each with checkbox steps.** Adapt to what the audit found — typical shape:

1. **Accounts and prerequisites** — sign up for the hosting platform and each production service; choose the cheapest tier that works, with monthly cost noted.
2. **Secrets and configuration** — create live API keys, set environment variables on the hosting platform, remove test keys. Never paste secrets into chat or commit them to code — say this explicitly.
3. **Production services** — database in production mode, auth configured for the real domain, payments switched from test to live (including webhooks and a real test purchase), email/AI/storage services on live credentials.
4. **Deploy the app** — the path of least resistance for the detected stack. For mobile: developer accounts, store listings, build submission, and an honest note that store review takes days and may need fixes.
5. **Domain** — buy/connect the domain, DNS setup, confirm HTTPS.
6. **Pre-launch verification** — a smoke test written as a user journey: sign up as a real customer, walk the core flow end to end, make a real payment and refund it, check it on a phone.
7. **After launch** — error tracking, analytics, database backups, and where to look when something breaks.

**Every step must have:**

- A checkbox, a 🧑/🤖/🤝 marker, and a time estimate.
- Plain-language instructions. Explain every technical term inline on first use — e.g. *"DNS (the address book that points your domain name at your app's server)"*, *"environment variable (a setting stored outside your code, used for secrets like API keys)"*, *"webhook (a way for one service to automatically notify another when something happens, like a successful payment)"*.
- For 🤖 steps: a ready-to-paste prompt for the coding agent, in a quote block.
- For 🧑 steps: exactly where to click/go, what they'll be asked for, and any cost.
- A "**You'll know it worked when...**" line so the founder can verify each step without guessing.

## Step 3: Walk the user in

Don't just drop the file. Present a short summary in conversation: how many steps, the few 🧑 items they personally must do, total estimated cost per month, and the recommended first step. Offer to execute the first 🤖 step now.

## Rules

- Be honest about cost, time, and risk — store review delays, DNS propagation ("can take up to a day"), payment provider verification.
- Recommend one path, not a menu. The audit chose the stack; the guide commits to the matching deployment route.
- Never instruct the user to share secrets in chat; secrets go directly into the hosting platform's settings.
- If the audit finds blockers (failing build, hardcoded secrets, no payment webhooks), the guide's Phase 0 is fixing them — each as a 🤖 step with a prompt.
- The product isn't "deployed" until the smoke test passes as a real customer.
