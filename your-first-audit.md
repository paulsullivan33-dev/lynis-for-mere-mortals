# Your First Audit

Outcome: one full audit run as root, its
summary read and believed at the right
level — which includes knowing which parts
of this lab's run were the machine, which
were the sandbox, and which were the
lab's own fault.

## The command

From the unpacked folder, as root:

```bash
./lynis audit system --quick
```

`audit system` is the local security scan.
`--quick` is the non-interactive mode: no
pauses, no questions, the audit runs start
to finish on its own. (The default profile
already sets `quick=yes`; the flag makes
the intent explicit and protects you on
systems whose profile differs.) Without
root, the same command works but sees
less — run it as root for the real thing.

The lab ran it with `--no-colors` added,
so the transcript would stay readable in
plain text:

```bash
./lynis audit system --quick --no-colors
```

## What the opening screen tells you

Before any test runs, Lynis states what it
is about to do and where the evidence will
land. From the lab's run (transcript t01):

```text
  Program version:           3.1.7
  Operating system name:     Ubuntu
  Operating system version:  24.04
  Kernel version:            7.0.0
  Hostname:                  htch-runtime
  Profiles:                  .../lynis-3.1.7/default.prf
  Log file:                  /var/log/lynis.log
  Report file:               /var/log/lynis-report.dat
  Auditor:                   [Not Specified]
```

Read this block every time. It catches
the two classic mistakes before they cost
you: auditing a different machine than you
think (wrong terminal, wrong host), and
reading last week's report file after this
week's audit wrote somewhere else.

The lab's run opened with one more thing —
a warning that a PID file already existed,
suggesting another Lynis process was
running. There wasn't one: an earlier
orientation command had left the file
behind. The audit continued normally.
Chapter 12 covers it; the lesson for now
is that Lynis warns and proceeds, and the
warning names its own most likely cause.

## While it runs

Tests scroll past grouped by subject —
boot services, kernel, accounts, file
permissions, firewalls, and the rest of
the menu from chapter 3. Each is quick.
The whole audit of the lab machine
performed **274 tests**.

How long should it take? On an ordinary
machine, a few minutes. The lab's run
took 7 minutes 21 seconds, and the report
file itself confesses where the time
went:

```text
slow_test[]=PKGS-7392,336.955439
```

One test — PKGS-7392, the check for
vulnerable packages — took 337 of those
seconds, because it first refreshes the
package lists (`apt-get update`), and this
sandbox's network stalled that refresh
almost to a standstill. The lab eventually
stopped the stuck `apt-get`; the test
carried on with the lists it had and still
completed. Two habits come out of this:

- A slow audit is usually *one* slow test,
  and the report names it in `slow_test[]`.
- The refresh is a profile choice
  (`refresh-repositories=yes` in the
  default profile). Chapter 7 turns it off
  in the lab's own profile — not because
  refreshing is wrong, but because when and
  how the package lists update is the
  system administrator's decision, not a
  side effect to inherit blindly.

## The ending that matters

The audit closes with the summary chapter
5 is built around. The lab's:

```text
  Hardening index : 67 [#############       ]
  Tests performed : 274
  Plugins enabled : 2

  Warnings (3):
  Suggestions (38):
```

Sixty-seven out of a hundred, three
warnings, thirty-eight suggestions — on a
disposable sandbox virtual machine that
nobody had ever tried to harden. That is
exactly the kind of machine this guide is
for practising on: the findings are real,
the stakes are zero, and nothing in the
next three chapters changes a single
setting on it.
