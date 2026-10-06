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


## 2026-10-06 · Week 1 Mon
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
