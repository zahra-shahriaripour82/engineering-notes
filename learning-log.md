## 2026-10-03 · Week 1 Sat

- Learned: a user story needs "so that"; acceptance criteria must be testable
- Confused: how small a story should be
- Mistake I won't repeat: putting 3 actions in one story

## 2026-10-04 · Week 1 Sun

- Learned: features = what the app does; quality attributes = how well
- Learned: a good scenario has a number or yes/no check
- Learned: JOIN matches rows by ID; LEFT JOIN keeps rows with no match
- Learned: microtasks (promises) run before macrotasks (setTimeout)
- Confused: write quality attributes was hard for me
- Next step: Day 3, the ERD (database diagram)

## 2026-10-05 · Week 1 Mon

- Learned: an ERD shows tables, columns and how they connect (PK, FK)
- Learned: a many-to-many link needs a middle table (coach_session_types)
- Learned: the role lives on memberships, so one person can be a
  client at one gym and a coach at another
- Learned: constraints are rules the database enforces even if the app
  has a bug (UNIQUE, CHECK, FOREIGN KEY)
- Double-booking: a partial UNIQUE index on (coach, starts_at) WHERE
  status = 'booked'. If two clients book the same slot at once, the
  database accepts the first and rejects the second. Weakness: it
  doesn't catch overlapping sessions with different start times.
  That gets fixed in week 9.
- Confused: designing the ERD from scratch felt too hard; I used the
  reference version
- Next step: Day 4, module map + ADRs + event-loop puzzles## 2026-10-06 · Week 1 Wed

## 2026-10-06 (Week 1, Tue)

- Module map: CoachSlot has 7 modules, like gym departments. Each table has
  exactly one owner module, and dependencies only go one way, with no circles.
  Booking depends on availability, catalog, organizations, notifications and audit.
- ADRs: an ADR records one decision with Context, Decision and Consequences,
  so a future developer knows WHY we chose it. One file per decision.
  - ADR-001: modular monolith, because it is one app that is simple to run, with clear module borders.
  - ADR-002: store times in UTC plus each gym's IANA time zone, because of daylight saving time.
  - ADR-003: server sessions instead of JWT, because a removed coach must lose access
    immediately. The cost is one database lookup per request.
- Event loop: Node has one thread. The order is: sync code, then ALL microtasks
  (Promise.then, code after await, queueMicrotask), then ONE setTimeout, then microtasks again.
  setTimeout(fn, 0) does not mean "now".
- Hardest part today: Event loop

## 2026-10-07 / 10-07 (Week 1, Wed)

- Free slots: store times as minutes since midnight, loop in 30-min steps per range,
  and a slot is valid only if slot + 30 <= end (it must END before work ends).
- TypeScript narrowing: after `if (status === 'booked')` TS knows the exact type.
  assertNever(x: never) in a switch's default = compile error if I forget a status.
  Result<T, E> = { ok: true, value } | { ok: false, error }: "too late to cancel" is a
  normal answer, not a crash, and the caller must handle both.
- Stories vs ERD: every story hides facts the app must remember. If a fact has no
  table.column, it's a gap. Fixed 4: invites, password_resets, gym contact, memberships.status.
- Pattern: "a link that works once and expires" = token (unique) + expires_at + used_at.
- Some rules live in the DATABASE (unique token, no double booking), others in APP code
  (24h cancel rule, password length).
- Hardest part: stories vs ERD

## 2026-10-11 / 10-11 (Week 1, Sun)

- SQL Aggregation: `COUNT()` counts rows, while `GROUP BY` divides rows into groups before counting. Pattern: "how many of each?" = `GROUP BY ...` + `COUNT(...)`.
- `LEFT JOIN` + `IS NULL`: useful for finding records that have no related records. Example: clients who have never booked.
- `ORDER BY ... ASC/DESC`: controls result order. `ASC` = earliest/smallest first, `DESC` = latest/largest first. `LIMIT` restricts how many rows are returned.
- `LIKE`: pattern matching. `LIKE 'cancelled%'` means the value starts with `cancelled`; `%` means any number of characters.
- `DATE()`: extracts the date from a timestamp. `date_trunc()` can find the start of a period such as a week.
- SQL query pattern: `SELECT` = what I need, `FROM` = where it starts, `JOIN` = which tables I need, `ON` = how tables connect, `WHERE` = which rows, `GROUP BY` = how to group, `ORDER BY` = how to sort.
- Database constraints: the double-booking rule is enforced by a partial unique index. PostgreSQL correctly rejected a second `booked` booking for the same coach and start time.
- Step 2: completed the seed data, tested the double-booking rule, and wrote 10 business queries for CoachSlot.

Lesson: Don't memorize SQL as isolated commands. Think in patterns: connect tables → filter rows → group rows → aggregate → sort the result.

## Week 1 — Learning Log

- I learned that user stories help me see the big picture of the project and understand its requirements.
- I learned that an ERD helps me see the tables, relationships, and foreign keys before I start writing the database.
- I learned that database constraints can protect important business rules. For example, a unique index can stop a coach from having two active bookings at the same time.
- I learned that `GROUP BY` creates groups and `COUNT(*)` counts the rows in each group.
- One mistake/confusion I had was struggling with writing user stories and SQL queries. All of these topics were new to me, so I am still a little confused.

2026-10-13 (Week 2, Day 1)

- Team setup: templates and CONTRIBUTING live in the repo because the workflow is code too:
  it's versioned, reviewed in a PR, and everyone sees the same rules.
  The first PR has to be filled by hand because GitHub reads the template from main, not my branch.
- Issue first, then branch: the issue says WHY and WHEN IT'S DONE before any code exists,
  so I don't build extra, and `Closes #n` links the code to its reason.
- A good issue title says what I BUILD, not how I TEST it ("one error format", not "I get 404").
  "Depends on" = the minimum it needs, not what came before it.
- Walking skeleton = an empty but real app through every layer (web → API → database → CI → deploy).
  Build it first because the hardest bugs live between layers; fix them while the app is tiny.
- HTTP: 401 = not logged in (missing or invalid credentials); 403 = logged in but not allowed.
  POST is not idempotent, so a double click on "Book" is stopped by the double-booking index → 409.
- Hardest part: team setup
