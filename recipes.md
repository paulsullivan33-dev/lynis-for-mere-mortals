# Recipes

Each recipe is small on purpose: a
question, the command that answers it, and
the check that proves the answer. All of
them ran in the lab unless the recipe says
otherwise.

## Audit one corner of the machine

*You changed the SSH configuration and
want only the SSH verdict.*

```bash
./lynis audit system --tests-from-group ssh --quick
```

The group names are the ones from
`./lynis show groups` (chapter 3). The
lab's version of this recipe audited the
`firewalls` group alone and got chapter
6's warning in twelve seconds instead of
seven minutes (transcript t07). Beware
the neighbouring trap in the next recipe.

## Look up one test without auditing

*The report names a test ID and you want
its evidence, not a new audit.*

```bash
./lynis show details FIRE-4512
```

The ID goes last — anything after it makes
the command answer with silence
(chapter 6, transcript t03). The lines it
prints come from the most recent audit's
log, so this recipe only works where an
audit has already run.

## Prove a warning wrong

*A finding accuses the machine of
something you believe it has.*

Take the test's own measurement and repeat
it with the tool the test is about, as
chapter 6 did with `iptables -L -n` and
chapter 5 did with the apt sources file
(transcripts t04, t11). If your check
disagrees, you have three honest options:
fix the machine (the finding was right),
fix your understanding (the finding meant
something narrower than you read), or
record a skip with the evidence in the
comment (the finding is wrong *here*).
All three start with the hand check; none
starts with arguing at the screen.

## Run just one test — carefully

```bash
./lynis audit system --tests KRNL-5830 --quick
```

Single tests are for tests that stand
alone. Some tests consume an earlier
test's discovery, and asked to run alone
they **skip themselves** — the lab asked
for FIRE-4512 alone and was told "Great,
no warnings" while the log recorded
*Skipped test FIRE-4512 … Prerequisites
not met* (transcript t06). After any
single-test run, grep the log for
`Skipped test` before believing the
summary. A group run (recipe 1) is usually
what you actually wanted.

## Audit a Dockerfile instead

*A container image's ancestor can be
audited without building it.*

```bash
./lynis audit dockerfile Dockerfile
```

Documented, not run: the lab's chapters
audit a system, and no Dockerfile was part
of its fixture. The mode exists, reads the
file's instructions, and reports the same
way; it is named here so a reader with
containers knows the door exists, and
chapter 10's neighbours take it from
there.

## Make a new machine's first report

*Before a fresh server does any work,
photograph it.*

Run the full audit, save the report under
the machine's name and the date, and read
the warnings once. That first report is
the baseline every later diff (chapter 8)
compares against — and the cheapest moment
to discover that the image you installed
from ships with surprises.
