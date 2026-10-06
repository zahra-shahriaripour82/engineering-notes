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
