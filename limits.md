# Limits

What Lynis cannot do, stated as plainly
as the tool states its findings.

## It reads; it does not repair

Lynis changes nothing it audits. Every
warning in this guide is still true of
the lab machine, by design. A reader who
wants the tool that *fixes* wants
OpenSCAP's remediation mode or a
configuration manager — and should want
them knowingly, because automated fixing
of security settings is automated breaking
of whatever depended on them.

## It sees configuration, not reality

Lynis reads the files and asks the kernel.
It does not exploit anything, probe the
machine from the network, or test whether
a defence actually holds. An empty
iptables ruleset it catches (chapter 6);
a ruleset that looks sensible and leaks
through one forgotten rule, it will
approve in silence. "Lynis found nothing"
means *nothing in its checklist was
wrong*, which is a smaller claim than
"this machine is secure" — and PKGS-7388
(chapter 5) shows the checklist can also
be wrong in the other direction, reporting
a missing security repository that a
hand check found present in a newer file
format. Treat findings as strong leads,
never as verdicts, in both directions.

## The index is not a compliance verdict

The hardening index is Lynis's own
weighting. It maps to no standard, passes
no audit, and certifies nothing. It can
also be gamed by the person being
measured: skipped tests stop hurting the
score, so a rising index must always be
read beside the profile that produced it.
Chapter 7's `custom.prf` is in the lab
folder precisely so the lab's numbers can
be re-read with their tuning visible.

## Coverage has edges

Tests exist for what the project knows:
mainstream services, common packages, the
usual places. Software outside the list is
invisible rather than approved — a service
Lynis has no tests for produces no
warnings and no suggestions, just absence.
Likewise the skipped list is part of the
truth of any audit: the lab's run skipped
SSH-7408, the SSH-server settings test,
because this machine has no SSH server
configuration for it to read. The
`tests_skipped` line of the report is long
in every real audit; a result is only
meaningful next to what was *not* checked.

## The sandbox shaped this guide's run

This guide's evidence comes from one
disposable virtual machine (the report
itself records `vmtype=systemd-nspawn`):
no SSH server to speak of, no mail or web
stack, a network that stalls package
refreshes, and an owner who never intended
to harden it. Its 67 is a teaching
example. A production server's report will
be longer, stranger, and mostly about
software this sandbox never had. The
method — read, look up, verify by hand,
then fix or formally accept — is the part
that transfers.

## The report is a sensitive document

A Lynis report is a map of a machine's
weaknesses: vulnerable packages by name,
missing defences by test ID. That is
exactly what makes it useful to you and
to anyone else who reads it. Keep reports
where you keep the keys, share them like
credentials, and remember chapter 1's rule
runs both ways: do not produce them for
systems that are not yours, either.
