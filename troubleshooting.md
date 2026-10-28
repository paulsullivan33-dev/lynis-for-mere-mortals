# Troubleshooting

Symptom first, cause second. Every entry
here happened in the lab or follows
directly from its transcripts.

## The audit takes forever

*Minutes pass on one unmoving test.*

1. **It is probably one test, and it is
   probably PKGS-7392.** That test refreshes
   the package lists before checking them.
   On a machine whose repositories are slow
   or unreachable, the refresh stalls — the
   lab's took 337 seconds against a network
   that would not answer, and the report
   recorded it as `slow_test[]=PKGS-7392`.
2. **Decide who owns the refresh.** If your
   package lists are maintained by your own
   update routine, set
   `refresh-repositories=no` in your
   profile (chapter 7) and let the test use
   the lists on the machine.
3. **Check the log's timestamps.** The gap
   between *Performing test ID* and the
   next line names the slow test precisely;
   `--slow-warning` lowers the threshold at
   which Lynis itself complains.

## "Great, no warnings" — but you expected one

*The summary is clean and you do not
believe it.*

1. **The test was skipped.** This is the
   first suspect, always. Grep the log:
   `grep "Skipped test" /var/log/lynis.log`.
   The lab asked for FIRE-4512 alone and got
   a clean summary because the test skipped
   itself — *Prerequisites not met*: tests
   that consume an earlier test's discovery
   cannot run solo. Run the test's group
   instead.
2. **Your profile skips it.** The log says
   so in as many words: *Skipped by profile
   setting (skip-test)*. That is chapter 7
   working as designed — check whether
   "you" who designed it still agrees.
3. **You are reading an old report.** Every
   run overwrites `/var/log/lynis-report.dat`
   and `lynis.log` — including quick group
   runs. The lab overwrote its full audit's
   report within the hour. Check the
   `report_datetime_start` line before the
   findings.

## `lynis show details` prints nothing

*The command exits 0 and says nothing at
all.*

**Something follows the test ID.** The ID
must be the last word on the command line:
`lynis show details FIRE-4512 --no-colors`
answers with silence (transcript t03's
first attempt). Re-run with the ID last.
If it is still silent, the test did not
run in the most recent audit — the details
come from that audit's log, and a test
that was skipped leaves only its skip
lines.

## A warning says a PID file exists

*Lynis opens by warning that another
process is probably running.*

An earlier Lynis invocation ended without
cleaning up (the lab's came from an
orientation command run before the audit).
If you are sure no audit is running —
check the process list — proceed; the
audit continues past the warning. If one
*is* running, let it finish: two audits
writing one log file produce a diary
neither of them wrote.

## A warning you cannot reproduce by hand

*Your own check says the machine is fine.*

Believe your hands, then get curious. The
lab's PKGS-7388 reported no security
repository while `noble-security` sat in
`/etc/apt/sources.list.d/ubuntu.sources` —
the test's reading and the modern
`deb822` source format did not meet
(chapter 5, transcript t11). Findings
about *absences* ("can't find", "not
installed", "no rules") are claims about
where the test looked as much as about the
machine. Reproduce the measurement, read
the log lines for what was searched, and
only then decide whether to fix, skip with
the evidence in the comment, or report the
false positive upstream.

## `update info` says Status: Unknown

*You checked for a newer Lynis and got a
shrug.*

The check needs to reach the project;
through a proxy or a filtered network it
may learn nothing, and it reports Unknown
rather than guessing (transcript t05).
Compare the version from
`lynis show version` against the project's
site yourself. And if you arrived here
from older documentation's `--check-update`
flag: it is deprecated — the message tells
you its replacement, `lynis update info`.
