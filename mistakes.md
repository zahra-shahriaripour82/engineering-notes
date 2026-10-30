## SQL: forgot quotes around a date

Wrote: WHERE joindate > 2012-09-01
Fix: WHERE joindate > '2012-09-01'
Why: without quotes, Postgres reads it as math (2012 minus 9 minus 1)

## Security test checked the wrong thing

Wrote: test checks the answer is "Gym B"
Fix: test checks the answer is "Not found" with no data
Why: getting Gym B's data back means security FAILED

## Mixed up two ideas in a scenario

Wrote: "I change it in 12h places"
Fix: "I change it in 1 place"
Why: the question was how many places in the code, not the new value

## Event loop (2026-10-06)

- Snippet 4: I predicted T1 T2 T3 P-inside-T2, the real output was T1 T2 P-inside-T2 T3.
  Why: after EACH setTimeout callback, Node clears all microtasks before calling the next timer.
- Snippet 6: I predicted a b c, the real output was a c b.
  Why: the second .then (b) is only queued after a finishes. By then c is already waiting in the queue.
- Snippet 7: I predicted email sent before save done, the real output was save start, sync end, save done, after save, email sent.
  Why: code after await and .then are microtasks, so they run before any setTimeout.

## Free-slots script, AI-off (2026-10-07)

- I looped over "09:00" (text). `i++` on text gives NaN, so the loop ran once.
  Fix: turn times into minutes since midnight ("09:30" -> 570) and loop on numbers.
- I added `+ 30` to the start time, so 09:00 was missing.
  Fix: the first slot starts exactly when work starts.
- I used the start of the FIRST range and the end of the LAST range, so lunch (12:00-14:00) became bookable.
  Fix: loop over each working-hours range separately.
- "9:30" instead of "09:30": my `=== 0 ? "00"` trick only works for :00.
  Fix: String(n).padStart(2, "0") for hours AND minutes.

## Stories vs ERD (2026-10-07)

- My invites table had no id, no not null, no unique token, and commas between columns (DBML has none).
- I said the other stories were fine, but A2 (reset password) had nowhere to store the link.
  Lesson: read every Given/When/Then word by word. "works once" and "expires" are columns.

## SQL / Database — 2026-10-11

- I used `COUNT()`, `GROUP BY`, `LEFT JOIN`, `IS NULL`, `LIKE`, `ORDER BY`, `LIMIT`, `DATE()`, `date_trunc()`, and `INTERVAL` in queries.
- I understood that `COUNT()` counts rows, while `GROUP BY` divides rows into groups before applying aggregate functions.
- I learned that `LEFT JOIN` + `IS NULL` is useful for finding records that have no related records.
- I learned that `ORDER BY ... ASC/DESC` controls the result order, and `LIMIT` restricts the number of returned rows.
- I completed 10 SQL queries for the CoachSlot database and tested the double-booking constraint.
- The database correctly rejected a second active booking for the same coach at the same start time.
  Error: `duplicate key value violates unique constraint "bookings_coach_start_active_idx"`.

Lesson: Don't just make a query work. Understand what each SQL clause does and learn the reusable patterns behind real business questions.

## Team setup (2026-10-13)

- feature.md: no `---` lines around the settings, `notes:` inside the settings block,
  and my placeholder text ("your instruction") left in 3 sections.
- CONTRIBUTING.md: rules 4–6 were topics ("When a PR may merge"), not rules that say what to do.

## Splitting CS-1 (2026-10-13)

- Issue #7 created with the title `feat:` only, and #8/#9 not put in the milestone.
- AC3 title described the test ("I get 404"), not the work (one error format).
- AC3 "depends on #9", but it only needed the API (#7).

## CS-1 design doc (2026-10-14)

- My Problem was a wish ("I want a fresh clone and web app...") instead of a problem.
  Lesson: a Problem answers 3 things: what's missing, who feels it, how we'll know it's solved (the ACs).
- My first options didn't answer the same question: "monorepo vs separate repo" (the ticket had
  already decided it) and "Railway vs free host" ("free host" isn't one specific thing).
  Lesson: 2 real, specific options that answer the same still-open question.
- I compared hosts on autoscaling (it doesn't matter with almost no users) and wrote a Render
  price ($1/month) that isn't in their docs.
  Lesson: criteria come from my situation; every number comes from a page I can link.
- I chose Render but pasted Railway's secrets, migrations and "Revisit when" lines, and wrote
  "Choice: B (Railway)".
  Lesson: never paste a line I can't explain. Pasted text must be adapted to MY choice.
- I copied the preview instead of the raw Markdown, so all the formatting broke. My PR said
  "tables render" before I had checked.
  Lesson: do the check first, then write it in "How I tested it". Upload files, don't copy previews.
- I merged with a merge commit, so main got "Add files via upload" in its history.
  Lesson: use Squash and merge, with a clear title.
- I skipped the AI-on review sheet for Decisions 3 and 4.

## Week 2 Day 3: monorepo, Drizzle, /health (2026-10-17)

- I created pnpm-workspace.yaml outside the repo root, so git said "nothing to commit".
  Lesson: when git doesn't see a file, check where I am: `git rev-parse --show-toplevel` and `ls`.
- The lockfile didn't include apps/api and apps/web until I ran `pnpm install` at the root.
  Lesson: in a workspace, always install from the root. There is only one lockfile.
- I copied the Drizzle docs example (integer id, length 255) instead of translating MY SQL.
  Lesson: the docs show the syntax; my SQL file decides the content.
- My editor auto-imported from `drizzle-orm/gel-core` and `mysql-core`, which caused the
  "MySqlColumn ... PgTable" error.
  Lesson: after every auto-import, check the import line. Postgres = `drizzle-orm/pg-core` only.
- I used `timestamp()` without `{ withTimezone: true }`, so it would have been `timestamp`, not `timestamptz`.
  Lesson: compare type by type with the SQL, and only then say "done".
- In memberships: wrong role values, a missing status column, and `unique` inside the columns.
  Lesson: constraints that use 2+ columns go in the third argument `(t) => [ ... ]`, in ONE list.
- My hand-written db/001-schema.sql had a missing comma in bookings, and I never noticed,
  because I never ran the file again after editing it.
  Lesson: run SQL after every change. If it never ran, it isn't done.
- I didn't know my AC5 issue number (#12).
  Lesson: keep the issue list open while writing the PR, or type `#` in the description to search.
- I almost ticked "503 with wrong password" without doing it.
  Lesson: "How I tested it" lists only what I actually did.

## week 2 Day 4

Testing: I needed to distinguish between writing e2e tests, running them, and manually verifying the API with curl. Test descriptions should reflect what I actually ran and verified.

Lint: Oxlint reported a warning about converting req.id to a string. I need to understand the request ID's actual type and fix the underlying type issue rather than blindly suppressing the warning.

Error handling: I learned why unexpected 500 errors should return a generic message to clients while the actual error details remain in server logs.

NestJS filter registration: I learned to register a global exception filter using APP_FILTER so it is also available when tests create the application without running main.ts.

ESM imports: I learned that this project's import paths use .js extensions even when the source files are TypeScript files.

## Day 5: Ubuntu setup, local Postgres, CI, deploy (2026-10-21 to 2026-10-24)

- After cloning on Ubuntu, /health said database:down because the new machine had no
  database for the project. I first tried Neon's "dev" branch, which didn't exist there.
  Lesson: a fresh clone has code but no .env and no data. Setting up a new machine is its own step.
- "url: undefined": drizzle-kit couldn't see DATABASE_URL. I was running commands from the wrong
  folder, and .env wasn't where I thought it was.
  Lesson: check `pwd` and `ls -a` first. .env must be in the folder where the command runs (apps/api).
- I spent a long time on Neon timing out from my laptop (TCP open, TLS silent, MTU tests).
  It never got fixed.
  Lesson: time-box network debugging. After ~1h, switch to the plan B (local Docker) and move on.
  Render reaches Neon fine, so it was my network, not my code.
- A long `node -e "..."` command broke when I pasted it.
  Lesson: put debug code in a small file (check-db.cjs) and delete it afterwards.
- "permission denied ... docker.sock": my user wasn't in the docker group, and VS Code's
  terminal still had the old groups after `usermod`.
  Lesson: after `usermod -aG docker $USER`, log out or restart (VS Code too). Use sudo only as a temporary workaround.
- ci.yml: `run: pnpm --filter |`, wrong script names, "2" to mean "run twice", a stray "yaml" line,
  wrong indentation.
  Lesson: YAML is spaces and structure. `run: |` then one command per line. Use the exact
  script names from package.json. Validate YAML before pushing.
- The docker-compose password was hardcoded, and it would have gone to GitHub.
  Lesson: read secrets with `${VAR}` from .env. Commit .env.example with fake values only.
- Render build: `npm install -g pnpm` failed with EROFS (read-only file system).
  Lesson: on a hosting platform I don't own the system folders. Use `npx`.
- Then npx failed with EBADDEVENGINES, because package.json says "use pnpm" (devEngines).
  Lesson: read the error name. EBADDEVENGINES means "wrong package manager", and `--force`
  makes it a warning.

  Lesson: next time, fill in at least the parts I know (what, why, how I tested) before asking.


 
## Day 6: Nest module from memory, NeetCode, curl -v (2026-10-25 to 2026-10-26)

I forgot using constructor for reading services.
 
- [ ] I didn't remember how Drizzle returns one row. `select ... where` returns an ARRAY,
  so I had to take `[0]` (or destructure `const [gym] = ...`).
  Lesson: Drizzle select is always a list. An empty list means "not found", which means throw NotFoundException.
- [ ] `insert` didn't return the new row until I added `.returning()`.
- [ ] `GET /gyms/not-a-uuid` returned 500, not 400.
  Lesson: my error filter only handles 404. Every other HttpException becomes 500. Fix this in Week 3.
- [ ] curl -v: I mixed up which lines were TCP and which were TLS.
  Lesson: "Connected to" = TCP done. "TLS handshake" / "SSL connection using" = TLS.






## Week 3, Day 1: vague ticket, PO questions, stories, threat model (2026-10-28 to 2026-10-30)

- I ran `test:2e2` instead of `test:e2e`.
  Lesson: when pnpm says "recursive exec failed", first check the script name in package.json.
- My first 5 product-owner questions were copied from the guide's examples.
  Lesson: choosing the questions IS the skill. Method: pick a person + a moment, ask what should happen,
  keep it only if the answer changes what I build.
- I didn't see that O1 ("This email already has an account") and A1 ("Email or password is incorrect")
  disagree: one hides who has an account, the other tells.
  Lesson: check every message an attacker can see on ALL pages (sign-up, login, forgot password).
- I first rate-limited sign-up "per email". The attacker tries many emails, so that never triggers.
  Then for login I had to count the opposite way: per email, because the attacker targets one account.
  Lesson: count by whatever stays the SAME for the attacker.
- My A3 story mixed "click Log out" and "logged out after 15 minutes": two triggers, two stories.
  Lesson: one trigger = one story.
- "I want to log in again" / "then log out my account" / "then can log in": not real wishes, not testable.
  Lesson: "I want" = the user's real wish; "then" = a page, a message, a number or a status I can check.
- Threat model: I put "check the role" as the defense for a stolen session.
  Lesson: a role check can't tell the real coach from someone using her session. Ending the session can.
- Threat model: I wrote what to do AFTER a database leak (shut down, tell users).
  Lesson: a threat model asks what to do BEFORE, so the leak hurts less (argon2id hash + salt).
- I wrote the stories and threat model only in the chat, so `git commit` said "nothing to commit".
  Lesson: work in the real files in the repo from the start.
- When I felt time pressure, I asked "complete it for me" (A4, A5, threat rows).
  Lesson: the goal is learning, not finishing. Stop for the day, or ask and then explain it back.