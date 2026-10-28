# The Neighbours

Lynis sits in a crowded field, and the
honest differences are mostly about
*where the tool stands* while it looks,
and *whose checklist* it reads from. The
tools below are characterised from their
documentation, not benchmarked in the lab;
the lab ran Lynis and only Lynis.

## OpenSCAP

The closest relative. OpenSCAP also audits
a machine from the inside, but its
checklists are the formal compliance
baselines — the Security Content
Automation Protocol content for standards
like PCI DSS and the DISA STIGs — and it
can, unlike Lynis, *apply* fixes with its
remediation mode. Choose OpenSCAP when an
auditor will ask which named standard you
meet; choose Lynis when you want to know
what a practitioner would fix first, in
language a person reads for pleasure.
They coexist happily: many systems run
both.

## CIS benchmarks and CIS-CAT

The Center for Internet Security publishes
benchmarks — long, prescriptive hardening
documents per operating system — and
CIS-CAT, the tool that scores a machine
against them. A CIS score answers "do we
match the document?" with authority.
Lynis's checks overlap the benchmarks
considerably (its controls pages often
read like a friendlier rendering of the
same advice) but its index is its own
invention, not a compliance verdict, and
chapter 11 insists on the difference.

## Trivy

A vulnerability scanner (see its guide in
this series): it matches your packages,
container images and configuration against
databases of *known vulnerabilities*.
That is a different question from Lynis's.
Trivy asks "is anything installed here
already known to be broken?"; Lynis asks
"is this machine arranged the way a
careful person arranges one?" The lab's
own report shows the two shaking hands —
Lynis's PKGS-7392 warning about vulnerable
packages is Trivy's home turf arriving
inside a Lynis report. A serious setup
runs both.

## Nessus, OpenVAS, and the network side

Network vulnerability scanners stand
*outside* the machine and probe it: open
ports, service banners, known flaws
visible from the wire. They see what an
attacker sees, which Lynis never claims
to — and they guess at much that Lynis
simply reads. An outside scan and an
inside audit disagreeing is not a
contradiction; it is two viewpoints, and
the interesting findings live in the gap.

## The distribution's own tools

Ubuntu's `ua` / Pro status, Debian's
`debsecan`, `systemd-analyze security`
(the lab's BOOT-5264 suggestion points at
it): narrow, official, and worth using for
their one question each. Lynis's value is
breadth in one sitting, not depth in any
single one.

## Lynis Enterprise

CISOfy's commercial tier: the same audits,
plus a central system that receives
uploads from many machines, compliance
dashboards, and support. Everything in
this guide is the free tool; the
Enterprise difference is *plumbing for
fleets*, not better eyes on one machine.
