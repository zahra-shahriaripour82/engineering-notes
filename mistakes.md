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
