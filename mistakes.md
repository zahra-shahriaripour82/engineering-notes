## SQL: forgot quotes around a date

Wrote: WHERE joindate > 2012-09-01
Fix: WHERE joindate > '2012-09-01'
Why: without quotes, Postgres reads it as math (2012 minus 9 minus 1)
