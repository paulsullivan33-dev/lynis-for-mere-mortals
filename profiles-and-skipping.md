# Profiles, Skipping, and Tuning

Outcome: a profile of your own that skips
one understood finding, leaves the
repository refresh to you, and stamps your
name into the report — each change proven
by a run, and each skip visible in the log
as a decision rather than a disappearance.

## What a profile is

A profile is a text file of `key=value`
lines. The audit uses `default.prf` from
the Lynis folder unless `--profile` names
another; the project also honours a
`custom.prf` placed alongside it (the
audit's closing tip suggests exactly
that). `lynis show settings` prints the
*effective* settings of the profile in
force — the lab's include
`machine-role=server`, the setting that
tells tests to judge the machine as a
server rather than a workstation, and
`error-on-warnings=0`, which is why an
audit with warnings still exits 0.

The default profile is worth reading
before you write your own. It is mostly
comments, and the comments are a catalogue
of everything tunable — including the
skip syntax this chapter uses:

```text
#skip-test=SSH-7408
#skip-test=SSH-7408:loglevel
```

The second form is the refined one: skip
*one check inside* a test (here, the SSH
daemon's log-level setting) while the rest
of the test still runs.

## The lab's profile

The lab wrote `custom.prf` with two
decisions in it, each with its reason —
write the reason in the file, because in
six months the file is all that remembers
(the full file is in `lynis-lab/`):

```text
# Skip the empty-iptables warning: this sandbox has no firewall
# rules by design, and the finding is already understood.
skip-test=FIRE-4512
# Do not refresh package repositories during an audit
refresh-repositories=no
```

The first is chapter 6's decision made
permanent. The second undoes chapter 4's
five-minute stall: PKGS-7392 will use the
package lists already on the machine
instead of refreshing them as a side
effect of being audited.

## Proving it

Same command as chapter 6's group run,
plus the profile and an `--auditor` name
(transcript t08):

```bash
./lynis audit system --tests-from-group firewalls \
  --quick --no-colors --profile ../custom.prf --auditor "Guide Lab"
```

The firewalls group, which an hour earlier
produced one warning, now reports:

```text
  Great, no warnings
```

and the evidence that this is tuning and
not blindness is in the two records:

```text
$ grep -E "^auditor|warning" /var/log/lynis-report.dat
auditor=Guide Lab

$ grep -A1 "Skipped test FIRE-4512" /var/log/lynis.log
Skipped test FIRE-4512 (Check iptables for empty ruleset)
Reason to skip: Skipped by profile setting (skip-test)
```

The warning is gone from the report; the
skip is written in the log with its
reason; and the report now names the
auditor, which is what `--auditor` is for
when reports from several people or
machines collect in one place.

## The discipline

A skip is a claim: *I have read this
finding, I understand it, and it does not
apply here* — or *it applies and I accept
it*. Both are respectable. What is not
respectable is skipping to make the
report quieter or the index higher: the
log keeps the reason either way, and
chapter 11 counts score-chasing among the
tool's real limits. Keep skips few, keep
their comments honest, and re-read them
whenever the machine's job changes — a
skip written for a sandbox follows the
profile, not the machine's new life.

## Keeping Lynis itself current

An auditor with old checks gives old
answers. The command is:

```text
$ ./lynis update info

  Version            : 3.1.7
  Status             : Unknown
  Release date       : 2026-06-25
```

(transcript t05). Two lessons in that
short output. First, the status was
**Unknown**, not "up to date": from this
sandbox's proxied network the check could
not confirm anything, and Lynis says so
instead of guessing — treat Unknown as
"check another way", for example against
the project's site. (The full audit's
report, on the same machine, recorded
`lynis_update_available=0`; the two checks
are not the same path, and where they
disagree the cautious reading wins.)
Second: older documentation tells you to
run `--check-update`. Doing so earns a
correction, also in transcript t05:

```text
This option (--info) is deprecated
Use: lynis update info
```
