# BCS Smart Topic Analyzer — Netlify deploy

## Deploy (2 minutes)
1. Push this folder to GitHub (or drag-drop is NOT enough — functions need a Git or CLI deploy: `npx netlify-cli deploy --prod`).
2. Netlify → Add new site → Import from Git. No build command; publish dir `public` (already in netlify.toml).
3. Site settings → Environment variables:
   - `GEMINI_API_KEY` (required) — from https://aistudio.google.com/apikey
   - `GEMINI_MODEL` (optional, default `gemini-2.5-flash`)
4. Redeploy. Open the site and run an analysis.

Local test: `npx netlify-cli dev` (put the key in a `.env` file, see `.env.example`).

## API (`POST /api/ai`)
| task | body | returns `data` |
|---|---|---|
| `analyze` | `{task, subject, topic}` | analysis object |
| `exam` | `{task, subject, topic, batch: 1-5}` | 10 questions (5 calls = 50) |

Errors: `{ok:false, error, message}` — codes: BAD_REQUEST 400, FORBIDDEN_ORIGIN 403, METHOD_NOT_ALLOWED 405,
PAYLOAD_TOO_LARGE 413, RATE_LIMITED 429, SERVER_NOT_CONFIGURED 500, UPSTREAM_ERROR 502, UPSTREAM_TIMEOUT 504.

## If exams time out
Netlify's default function timeout is 10s. Raise it to 26s: Site settings → Functions → timeout.

Optional speed-up: set GEMINI_THINKING_BUDGET=0 (only for gemini-2.5-flash).
