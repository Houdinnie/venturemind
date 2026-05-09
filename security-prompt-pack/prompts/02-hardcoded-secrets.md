# Security Prompt Pack — Section 02
## Hardcoded Secrets & API Keys

**Vulnerability:** AI tools put passwords and keys directly in the code — sometimes in frontend files visible to every visitor. The correct approach: environment variables. The real value lives in a `.env` file that never gets committed to Git.

**VentureMind risk:** All 17 GitHub integrations (OpenFang, Paperclip, AgentGPT, etc.) use API keys.

---

## PROMPT 1 — Scan for hardcoded secrets

```
Scan this entire project for any hardcoded API keys, passwords, database URLs,
tokens, webhook secrets, or other sensitive values.
Check every file — including config files, test files, utility files, and comments.
For each one you find, tell me:
(1) what file it's in
(2) whether it's a frontend or backend file
(3) how to move it to an environment variable
Anything in a frontend file is especially critical because it's visible to every visitor.
```

**Expected output:** Table of secrets found. Frontend = CRITICAL. Backend = HIGH.

---

## PROMPT 2 — Set up environment variables correctly

```
Move all secrets in this project to environment variables.
Create a .env file with clear, descriptive variable names
(like OPENAI_API_KEY, SUPABASE_URL, STRIPE_SECRET_KEY).
Update every file that references these values to use the environment variable instead.
Make sure .env is in .gitignore.
Create a .env.example file with the variable names but no actual values,
so other developers know what's needed.
```

**Expected output:** `.env` created, all secrets migrated, `.gitignore` updated, `.env.example` created.

---

## PROMPT 3 — Check for secrets in frontend code

```
Search every frontend file in this project —
anything that runs in the browser
(React components, Next.js pages, client-side scripts, public folders) —
for any API keys, tokens, or secrets.
These are visible to every visitor via the browser's developer tools
and must be moved to backend API routes or server-side functions immediately.
List every instance you find.
```

**Expected output:** Any frontend secret = immediate migration to backend-only route.

---

## PROMPT 4 — Verify .gitignore and Git history

```
Check if .env is in .gitignore.
Then check the full Git commit history to see if .env
or any file containing secrets was ever committed — even if it was later removed.
Use:
  git log --all --full-history -- .env
  git log --all -p --diff-filter=A -- '*.env'
If any secrets were ever committed, list them so I can rotate those keys immediately.
```

**Expected output:** `.env` in `.gitignore` = pass. Git history clean = pass. Any committed secret = rotate immediately.

---

## PROMPT 5 — Separate public vs secret keys

```
Review every API key and token in this project.
Some services have two types of keys:
- public/publishable keys (safe in frontend code)
- secret keys (must never be in frontend code)
For each key, tell me:
(1) which service it belongs to
(2) whether it's a public or secret key
(3) whether it's currently in a frontend or backend file
Move any secret keys that are in frontend files to backend API routes immediately.
```

**Expected output:** All keys classified. Secret keys in frontend = migrated immediately.

---

## PROMPT 6 — Handle API keys safely with AI tools

```
I need to add [SERVICE NAME] to this project.
Set up the integration using an environment variable for the API key.
Store the variable name in .env with a placeholder value.
Never put the actual key value in any code file.
Show me where in my hosting platform settings I'll need to add the real value for production.
Remind me to never paste the actual key into this chat.
```

**Expected output:** Integration uses `process.env.SERVICE_KEY` only. No real values in code.

---

## CRITICAL RULE

> **Never paste your actual API key values into your AI tool's chat window.**
> The AI only needs the variable name (`process.env.MY_API_KEY`) to write the code.
> You add the real value to your `.env` file yourself.