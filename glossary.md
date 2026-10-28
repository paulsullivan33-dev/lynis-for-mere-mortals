# Glossary

**Audit** — one full Lynis run over a
machine: every applicable test, then the
report (chapter 4).

**Auditor** — the name stamped into a
report with `--auditor`, saying who ran
the audit (chapter 7). Unset, the report
says `[Not Specified]`.

**Control** — the CISOfy web page for one
test ID (`cisofy.com/lynis/controls/…`),
giving the finding's background and
advice; every warning on screen links to
its control.

**Group** — a bundle of tests on one
subject (`ssh`, `firewalls`, `kernel`),
runnable on its own with
`--tests-from-group` (chapters 2, 9).

**Hardening** — the work of making a
system harder to misuse: closing what is
open, setting what is lax, removing what
is unneeded. Lynis reports on it; it does
not do it.

**Hardening index** — Lynis's score out of
100 for one audit. A comparison number
between runs of the same machine, not a
compliance verdict (chapters 5, 11).

**Log file** — `/var/log/lynis.log` by
default: the complete diary of an audit,
including every skip and its reason.

**Plugin** — an extra test set loaded from
the `plugins/` folder; its test IDs start
with PLGN (chapter 2).

**Profile** — a text file of options
(`default.prf`, or your own via
`--profile`) tuning an audit: skips,
machine role, refresh behaviour
(chapter 7).

**Quick mode** — `--quick`: the audit runs
non-interactively, start to finish. The
mode for scripts and schedules.

**Report file** — `/var/log/lynis-report.dat`
by default: the machine-readable
`key=value` record of an audit — findings,
facts, and the index (chapters 5, 8).

**Security repository** — the package
source carrying security updates
(`noble-security` in the lab machine's
case); test PKGS-7388 checks for one.

**Skip** — a test deliberately not run,
recorded in the profile (`skip-test=`)
and always confessed in the log with its
reason (chapter 7).

**Suggestion** — a finding Lynis judges an
improvement rather than a fault; the long,
calmer list under the warnings
(chapter 5).

**Test** — one check, with one **test ID**.

**Test ID** — the permanent name of a
test: a subject prefix and a number, such
as `FIRE-4512` or `SSH-7408`. The key to
looking a finding up, discussing it, and
skipping it.

**Warning** — a finding Lynis judges
wrong: the short list at the top of the
results, each with its test ID and control
link (chapter 5).
