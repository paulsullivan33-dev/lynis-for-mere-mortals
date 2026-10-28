# Lynis for Mere Mortals

A system keeps every decision anyone ever
made on it: the service left running, the
setting nobody remembers, the permission
set wide because it was easier that day.
**Lynis** is a program that goes and
reads those decisions. It runs several
hundred small checks on the machine it is
executed on — accounts, kernel, files,
firewall, software, logging — and reports
**warnings**, **suggestions**, and one
summary number, the **hardening index**.
Every finding carries a **test ID**, so
every finding can be looked up, verified,
discussed, or deliberately set aside.

This guide explains how to run that
audit, how to read what comes back
without either panicking or worshipping
the score, how to take one warning apart
from headline to hand-checked fact, and
how to tune the audit with a **profile**
so its report stays worth reading month
after month. The limits are stated
plainly, including the founding one:
Lynis repairs nothing. It tells you where
to look; the looking, and then the fixing,
are yours.

The rule that governs everything here is
stated in chapter 1: **audit only systems
that are yours — machines you own, or
systems whose owner has given you explicit
permission to audit.** A Lynis report is a
map of a system's weaknesses, and a map
like that belongs to the system's owner.
The lab obeyed the rule the strict way:
it audited one disposable sandbox machine,
and it applied *none* of the fixes the
audit suggested.

The lab behind this guide ran the real
thing: **Lynis 3.1.7**, unpacked from the
project's own archive on an Ubuntu 24.04
virtual machine, run as root against that
machine itself. The audit performed 274
tests and returned a hardening index of
67, three warnings, and thirty-eight
suggestions. The lab read one warning
(FIRE-4512, the empty firewall) down to
the tool's own evidence and then verified
it by hand with `iptables`; it checked a
second warning (PKGS-7388) by hand and
found the accusation wrong on that
machine; it watched one test stall for
five minutes on a stuck package refresh
and another test silently skip itself
when run alone under a "no warnings"
summary; and it proved a two-line profile
could retire an understood finding while
the log kept the reason. What the lab
could not do, it did not fake: no
remediation of any kind was performed —
no settings changed, no packages
installed — and scheduling, package
installs, Dockerfile audits, and the
Enterprise upload are taught from the
documentation and labelled as such.
`criteria.md` keeps the full ledger.

## What this guide covers

- [**What Lynis is**](what-lynis-is.md) —
  the mental model: an auditor that reads
  a system and changes nothing, findings
  with test IDs, and the permission rule
  that is part of the tool
- [**The moving parts**](the-moving-parts.md) —
  the script, its test modules, groups,
  plugins, profiles, and the three outputs
  (screen, log file, report file)
- [**Installing Lynis**](installing-lynis.md) —
  the unpack-and-run route the lab
  verified, the distribution package
  route, and the two orientation commands
- [**Your first audit**](your-first-audit.md) —
  the full run as root, its opening
  screen, and why the lab's took seven
  minutes (one slow test, named by the
  report itself)
- [**Reading the results**](reading-the-results.md) —
  warnings versus suggestions, what the
  hardening index means and doesn't, and
  why the screen and the report file
  count differently
- [**One finding, end to end**](one-finding-end-to-end.md) —
  FIRE-4512 taken apart: the evidence, the
  hand check with `iptables`, what fixing
  it would involve, and the honourable
  alternative of accepting it on record
- [**Profiles, skipping, and tuning**](profiles-and-skipping.md) —
  your own profile: skipping an understood
  test, owning the repository refresh,
  naming the auditor, and keeping Lynis
  itself current
- [**Auditing again**](auditing-again.md) —
  the recurring audit: keeping dated
  reports, diffing two of them, and the
  scheduled pattern assembled from
  verified pieces
- [**Recipes**](recipes.md) — audit one
  corner, look up one test, prove a
  warning wrong, run a single test
  carefully, and photograph a new machine
- [**The neighbours**](the-neighbours.md) —
  OpenSCAP, the CIS benchmarks, Trivy,
  the network scanners, and Lynis
  Enterprise: who stands where, and whose
  checklist they read
- [**Limits**](limits.md) — reads but
  never repairs, configuration rather
  than reality, an index that is not a
  compliance verdict, and the report as a
  sensitive document
- [**Troubleshooting**](troubleshooting.md) —
  the endless audit, the clean summary you
  shouldn't believe, the silent
  `show details`, the PID-file warning,
  and the warning your hands disprove
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Hardening itself. This guide teaches the
audit and the reading of it; applying
fixes — firewall rules, service settings,
account policy — belongs to the guides
for those tools, several of them in this
series, and chapter 6 explains why that
separation is a feature. Auditing other
people's systems is not a harder chapter
of this book either: it is off the map
entirely, per chapter 1. Compliance
certification (PCI, CIS, STIG) is pointed
at in chapter 10, not taught; a Lynis
index passes no standard. Managing a
fleet through Lynis Enterprise is
likewise pointed at, not run.

## How to use this guide

Read chapter 1 for the model and the
rule, and chapter 2 before installing —
knowing which output is which turns later
confusion into a named problem. Chapter 4
runs the first audit in the same sitting
as the install, ideally on a machine you
can afford to learn on; do not start with
the production server's report at 2 a.m.
Chapter 6 is the chapter the rest of the
guide stands on: work one finding by hand
before you let any finding move you to
change anything. Keep the report of every
audit from the beginning — chapter 8's
comparisons are only possible for readers
who did. The lab's transcripts live in
`lynis-lab/transcripts/` and its profile
is `lynis-lab/custom.prf`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Trivy for Mere Mortals](https://paulsullivan33-dev.github.io/trivy-for-mere-mortals/)
- [Cybersecurity for Mere Mortals](https://paulsullivan33-dev.github.io/cybersecurity-for-mere-mortals/)
- [UFW for Mere Mortals](https://paulsullivan33-dev.github.io/ufw-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/lynis-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/lynis-for-mere-mortals/blob/main/LICENSE-CODE).
