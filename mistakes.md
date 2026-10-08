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
