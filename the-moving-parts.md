# The Moving Parts

Outcome: you can name the five pieces of
Lynis, say which one produces the report
file, and predict what a **profile**
changes before you ever edit one.

## The program

Lynis is one shell script, `lynis`, plus a
library it carries with it. There is no
daemon, no database server, no agent
waiting in the background. When you run
an audit, the script starts, does its
work, writes its files, and exits. The
unpack of version 3.1.7 the lab used
looks like this at the top level:

```text
lynis            the program itself
include/         the test modules and shared functions
db/              data files the tests compare against
plugins/         extra test sets, enabled by profile
default.prf      the default profile
lynis.8          the man page
```

The `include/` folder is where the audits
live: one file per subject —
`tests_ssh`, `tests_kernel`,
`tests_file_permissions`, and so on.
Because it is all shell script, a curious
reader can open the module for any test
and read exactly what it checks. Few
security tools are this inspectable.

## Tests and groups

The unit of work is the **test**: one
check, one **test ID** (`SSH-7408`,
`KRNL-5830`, `FILE-7524`). The prefix
names the subject — SSH tests start with
SSH, kernel tests with KRNL or
KERNEL-related codes, file tests with
FILE. Tests are bundled into **groups**
that match the module files:

```text
$ ./lynis show groups
accounting
authentication
banners
boot_services
containers
crypto
...
kernel
kernel_hardening
...
ssh
storage
```

A full audit runs every group that
applies to the machine. You can ask for
one group only (`--tests-from-group ssh`),
or named tests only (`--tests SSH-7408`),
which is how chapter 6 reads a single
finding without re-auditing the world.

## Plugins

Alongside the built-in tests, Lynis loads
**plugins** — extra test sets enabled in
the profile. The default profile enables
a long list of them (authentication,
compliance, crypto, docker, firewalls,
malware, PAM, and more). Plugins use IDs
with a different shape, such as
`PLGN-0010`. You do not need to manage
plugins to use Lynis; you need to know
they exist so a PLGN finding in a report
does not look like a misprint.

## Profiles

A **profile** is a plain-text file of
options that tunes an audit: which tests
to skip, which to run, how much to print,
what counts as the machine's role. Lynis
ships `default.prf`, and every audit uses
it unless you point `--profile` at your
own. Two lines from the default profile
show the flavour:

```text
machine-role=server
quick=yes
#skip-test=SSH-7408
```

The commented line is the interesting
one: remove the `#` and that test stops
appearing in your reports — because you
have *decided* something about it, which
is a very different thing from not having
seen it. Chapter 7 is about making those
decisions well.

## The three outputs

An audit produces three things, and
knowing which is which ends most beginner
confusion:

1. **The screen.** Progress and, at the
   end, the results summary: warnings,
   suggestions, and the hardening index.
   Coloured, readable, and gone when you
   close the terminal.
2. **The log file** (`/var/log/lynis.log`
   by default). The detailed diary: every
   test that ran, what it found, why a
   test was skipped. This is where you go
   when a result surprises you.
3. **The report file**
   (`/var/log/lynis-report.dat` by
   default). Machine-readable `key=value`
   lines: every warning and suggestion
   with its test ID, the hardening index,
   facts about the machine. This is the
   file you keep, diff between runs, and
   feed to other tools — chapter 8 builds
   the recurring-audit habit on it.

Check: after any audit, the last screen
of output names both file paths. If you
cannot find a finding again, it is in one
of those two files, under its test ID.
