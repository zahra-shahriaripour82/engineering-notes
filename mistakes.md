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
