# Reading the Results

Outcome: given any audit's three outputs,
you can find every warning, say what the
hardening index does and does not mean,
and explain why the screen and the report
file give different counts — because in
the lab's run, they did.

## Warnings and suggestions

The Results section lists **warnings**
first. The lab's three:

```text
  ! Can't find any security repository in /etc/apt/sources.list or sources.list.d directory [PKGS-7388]
  ! Found one or more vulnerable packages. [PKGS-7392]
  ! iptables module(s) loaded, but no rules active [FIRE-4512]
```

A warning is Lynis saying *this looks
wrong*, in the imperative mood, with a
test ID and a link to the control's page
on the CISOfy site. Below them come the
**suggestions** — thirty-eight in the lab
run — which are improvements rather than
accusations: configure password ageing,
enable auditd, put `/var` on its own
partition, add a legal banner. One test
can produce several suggestion lines
(AUTH-9286 suggested both a minimum and a
maximum password age), and one suggestion
can be mundane. Suggestions are a reading
list, not a rap sheet.

Neither list is gospel. Chapter 6 takes
one warning apart completely; but note
already that the first warning did not
survive hand-checking. The machine's
security repository exists — declared in
the newer `deb822`-format file
`/etc/apt/sources.list.d/ubuntu.sources`:

```text
Suites: noble noble-updates noble-backports noble-security
```

(transcript t11). The test reported what
it could not find in the places it looked;
the machine was less guilty than the
headline. *Verify one finding by hand
before you act on any of them* is the
whole method of this guide in one
sentence.

## The hardening index

The index is a weighted score out of 100
summarising how the machine did across
the audit — the lab machine scored
**67**. Used well, it is a *comparison*
number: the same machine, audited the
same way, before and after your work, or
month against month (chapter 8). Used
badly, it is a target: a score can be
raised by skipping tests, disabling
checks, and optimising for the ruler
instead of the system. The index does not
know what the machine is *for*. A file
server and a throwaway build box should
not have the same report, and a 67 on a
sandbox you are about to delete is a
fine score. Chapter 11 returns to this.

## Why the counts differ

The screen said 3 warnings and 38
suggestions. The report file, examined
afterwards, holds:

```text
$ grep -c '^warning\[\]' /var/log/lynis-report.dat
2
$ grep -c '^suggestion\[\]' /var/log/lynis-report.dat
21
```

Both are telling the truth about
different things. The report file is a
condensed machine record: it carries the
warnings and suggestions tests registered
into it (transcript t02), plus the facts —
`hardening_index=67`, every test executed
and skipped, the vulnerable packages by
name (`vulnerable_package[]=libbrotli1`
and friends). Some findings live only on
the screen and in the log: PKGS-7388's
warning, and suggestions such as
FILE-6310's separate-partition advice,
appear in the log's full diary — all 38
suggestions are there — but never became
`warning[]` or `suggestion[]` lines.

The working rule:

- **Screen** — for the audit you just ran.
- **Report file** — for keeping, diffing,
  and feeding to tools (chapter 8).
- **Log file** — for the complete record,
  and for settling any argument between
  the other two.

The report also quietly records things the
screen never showed: `vmtype=systemd-nspawn`
(it knew it was inside a container),
`slow_test[]` (chapter 4), and
`lynis_update_available=0` — Lynis checked
its own currency as part of the run.
