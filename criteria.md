# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab audited a disposable sandbox machine and nothing else.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** The Limits chapter names what the
   tool cannot do: it reads and never repairs, it sees
   configuration rather than tested reality, its index is no
   compliance verdict and can be gamed by skipping, its
   coverage ends where its test list ends, and its report is a
   sensitive document in its own right.
8. **The ethics rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *audit only systems that are
   yours, or whose owner has given you explicit permission* —
   before any procedure depends on it, and later chapters point
   back to it where the report's sensitivity makes it matter.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a disposable Ubuntu 24.04.5 LTS virtual
machine (a systemd-nspawn container, as the audit's own report
records). Lynis **3.1.7** — the current release, dated
2026-06-25 — was fetched as the source archive of the official
`CISOfy/lynis` project repository after the vendor download
link answered with an HTML page instead of the tarball, unpacked
into a scratch folder, and run from there. No package was
installed and no system configuration was changed at any point:
the lab audited, and deliberately did not harden. All
transcripts live in `lynis-lab/transcripts/`; the lab's profile
is `lynis-lab/custom.prf`.

Run for real, with transcripts as evidence:

- Version and orientation: `--version`, `show version`,
  `show groups`, `show settings` (t09), and the top-level help.
- A full audit as root, `lynis audit system --quick`
  (t01): 274 tests performed, hardening index 67, 3 warnings
  and 38 suggestions on screen; the report file it wrote is
  kept as t02. The run took 7m21s because PKGS-7392's
  repository refresh stalled on this sandbox's network — the
  report's own `slow_test[]=PKGS-7392,336.955439` line is the
  evidence — and the lab stopped the stuck `apt-get` so the
  test could finish with the lists it had.
- Both remaining warnings read in the log via
  `lynis show details` (t03), including the trap that an
  option placed after the test ID makes the command print
  nothing while exiting 0.
- Findings verified by hand: FIRE-4512's empty ruleset with
  `iptables -L -n` (t04 — all chains empty, policy ACCEPT),
  and PKGS-7388's "missing" security repository found present
  in `/etc/apt/sources.list.d/ubuntu.sources` (t11).
- A firewalls-group audit (t07), the same audit under the
  lab's profile with `skip-test=FIRE-4512` and
  `refresh-repositories=no` (t08): warning gone, log giving
  the reason "Skipped by profile setting (skip-test)", and
  `--auditor` landing in the report.
- A single-test run of FIRE-4512 (t06), which skipped itself
  ("Prerequisites not met") under a "Great, no warnings"
  summary — the guide's central cautionary transcript.
- `lynis update info` (t05): version and release date
  confirmed, update status honestly **Unknown** through this
  network; and `--check-update` answering with its deprecation
  notice.
- The report-comparison technique of chapter 8 (t10): the
  finding lines of two real reports extracted, sorted, and
  diffed.

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Every act of remediation. The lab changed no configuration,
  installed and removed no packages, and edited nothing under
  /etc. Chapter 6 describes what fixing FIRE-4512 involves
  and why it is work for a firewall guide, not for an audit.
- Distribution package installs (the lab never installs
  packages on its shared build machine) and CISOfy's own
  package repository.
- Unprivileged audits: described from the INSTALL notes (root
  preferred; an ordinary account sees less and the report
  shows the skips). All lab audits ran as root.
- Cron/systemd scheduling of audits (chapter 8): the audit,
  profile, report handling, and comparison in that chapter
  are all verified; the cron line itself is an assembly of
  those pieces, labelled as not run.
- `lynis audit dockerfile`, remote audits, and Lynis
  Enterprise uploads (`--upload`) — no Dockerfile fixture,
  second machine, or Enterprise system exists in this lab.
- The comparison tools in chapter 10 are characterised, not
  benchmarked.

Fixtures are this guide's own: one disposable sandbox machine
with no role beyond being audited, and one two-decision
profile written for it. No example is modelled on any real
person, network, service or organisation, and no system was
audited that the lab was not free to audit.
