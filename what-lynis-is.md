# What Lynis Is

A Unix system accumulates decisions. A
service left enabled from the install, a
setting nobody remembers choosing, a
permission set wide because it was easier
that day. None of them announce
themselves. **Lynis** is a program that
goes and looks: it runs several hundred
small checks against the machine it is
executed on — accounts, kernel settings,
file permissions, network services,
installed software, logging — and writes
down what it finds as **warnings** (things
it judges wrong), **suggestions** (things
it judges improvable), and a single number,
the **hardening index**, that summarises
how the machine did.

Lynis is written by CISOfy and released
as open source under the GPL. It is a
shell script, not a compiled program: it
installs nothing into the system,
starts no service, and phones nothing
home. You unpack it, run it, read the
report, and the only traces it leaves are
the report and log files it tells you
about. That shape matters for a security
tool: you can audit a machine without
first trusting the machine with a new
permanent resident.

Each check has an identifier — a
**test ID** such as `SSH-7408` — and the
report names the ID next to every finding.
That is the feature this guide leans on
hardest. A finding with an ID can be
looked up (`lynis show details SSH-7408`),
searched for in the project's
documentation, discussed with a colleague
without ambiguity, and deliberately
skipped in a profile when you have decided
it does not apply to you. Lynis is not a
pile of opinions; it is a pile of
*addressable* opinions.

## What an audit is, and is not

An audit is a reading, not a repair.
Lynis changes nothing on the machine it
examines — it never edits a configuration
file, never installs a patch, never
restarts a service. Every fix in this
guide is something *you* do afterwards,
with your own hands, understanding what
you are changing. That separation is the
tool's honesty and its limit in one: it
can tell you the door is unlocked; it
will not lock it, and it cannot tell you
whether locking it breaks the thing the
door was left open for.

It is also not a vulnerability scanner in
the network sense. Lynis runs *on* the
machine, with local eyes: it reads the
configuration files and asks the kernel,
rather than probing ports from outside and
guessing. Chapter 10 puts it next to the
tools that work the other way.

## The rule, stated before everything else

A tool that reads a whole system reads
things that are not yours to read on
somebody else's system: password policy,
account lists, configuration secrets,
the shape of a network. So this guide has
one rule, and it is not fine print:

> **Audit only systems that are yours —
> machines you own, or systems whose owner
> has given you explicit permission to
> audit.**

Your own laptop, your own server, the
machine at work you were hired to harden:
yes. A client's server "just to look", a
shared machine your account happens to be
on, a friend's computer while fixing
something else: no. The audit itself
changes nothing, but the *report* is a
map of a system's weaknesses, and a map
like that belongs to the system's owner.

The lab in this guide obeyed the rule the
strict way: it audited a disposable
sandbox machine and nothing else, and it
applied **none** of the fixes the audit
suggested. The score that machine earned
is an example to learn reading on, not a
target to beat.

## A first taste, after installing

Chapter 3 does the install; the shape of
the thing is this:

```bash
./lynis audit system --quick
```

One command, run as root, a few minutes
of checks scrolling past, and at the end
a summary: how many tests ran, how many
warnings and suggestions came back, and
the hardening index. Everything after
that — the report file, the details of
each test, the profile that tunes the
next run — is chapter 5 onwards.
