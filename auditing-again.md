# Auditing Again

Outcome: the one-off audit becomes a
habit — the same audit on a schedule, the
report kept each time, and two reports
compared so that *what changed* is a
three-line answer instead of a fresh
forty-finding read.

## Why audit twice

A single audit is a photograph. The second
audit is where the value starts: findings
you fixed should be gone, findings you
accepted should be the only ones left, and
anything new is a change in the machine —
yours, a colleague's, an update's — that
you did not know about. The hardening
index earns its keep here too: 67 last
month and 71 this month, on the same
machine with the same profile, is a
measurement. Either number alone is a
horoscope.

## The mechanism: keep the report

Chapter 5's report file is built for this.
It is plain `key=value` text, it lands in
the same place every run, and its finding
lines sort and diff cleanly. The lab
demonstrated the comparison on two real
reports — the full audit of chapter 4 and
a later run — with three commands
(transcript t10):

```bash
grep -E "^(hardening_index|warning|suggestion)" report-a.dat | sort > a.txt
grep -E "^(hardening_index|warning|suggestion)" report-b.dat | sort > b.txt
diff a.txt b.txt
```

Every line the diff prints is a finding
that appeared or disappeared between the
two runs, plus the index moving. In real
use both reports come from the *same*
full audit weeks apart, so the diff is
short enough to read over coffee — and
"nothing changed" is a diff with no
output, which is the best result an audit
can give.

Keep the reports with dates in their
names (`lynis-report-2026-10-28.dat`),
because the audit overwrites
`/var/log/lynis-report.dat` on every run —
including small ones. The lab's own
single-group runs replaced the full
audit's report within the hour; the full
version survives only because chapter 4's
copy was saved into the lab folder first.
Copy the report *before* you experiment.

## The schedule itself

The pattern is a cron entry (or a systemd
timer) running the audit as root, in
quick mode, with your profile, followed by
copying the report somewhere dated:

```cron
15 4 * * 1  root  cd /opt/lynis && ./lynis audit system --quick --profile /etc/lynis/custom.prf && cp /var/log/lynis-report.dat /var/log/lynis-reports/lynis-report-$(date +\%F).dat
```

This exact line is **documented, not run**:
the lab does not install cron jobs on its
shared build machine, and the guide will
not pretend otherwise. Every part of it
except the scheduling was run for real in
earlier chapters — the audit, the profile,
the report, the comparison — so the cron
line is an assembly of verified pieces,
and your first scheduled run should be
checked against chapter 4's manual one.

Two settings earn their place in a
scheduled life. `error-on-warnings` in a
profile can make the audit exit non-zero
when warnings appear, which is how a
scheduler or a monitoring script notices
without parsing anything. And Lynis
Enterprise — the commercial sibling — can
receive uploads from audits (`--upload`),
which is CISOfy's answer to doing this
across a fleet; it is documented-only
here, as the lab has no Enterprise system
to upload to, and chapter 10 notes it
among the neighbours.
