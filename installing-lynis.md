# Installing Lynis

Outcome: Lynis unpacked (or installed from
your distribution), its version printed,
and the difference between the routes
understood — because with Lynis the
"install" is unusually honest: the tool is
a shell script, and the simplest route
barely installs anything at all.

## Route one: unpack and run

Lynis is distributed as a tarball by
CISOfy. Unpack it anywhere you like, step
inside, and the program is ready:

```bash
tar xzf lynis-3.1.7.tar.gz
cd lynis-3.1.7
./lynis --version
```

The lab took this route — with one wrinkle
worth recording. The direct download link
for the release tarball on the vendor's
site answered with an HTML page instead of
the archive, so the lab fetched the same
release (3.1.7, the current release) as
the source archive of the official project
repository on GitHub, `CISOfy/lynis`. Same
program, same version; check the version
line whichever route you take:

```text
$ ./lynis --version
3.1.7
```

Nothing was copied into system folders,
no package database learned Lynis's name,
and removing it later is deleting the
folder. For a tool you run a few times a
year, that is often the right amount of
installation. Root privileges are not
needed to *unpack*, but chapter 4 runs
the audit as root: many tests read files
and settings that an ordinary account
cannot see, and Lynis says plainly in its
INSTALL notes that root is preferred for
full audits. Run unprivileged, it still
works — it simply sees less, and its
report says which tests were skipped for
lack of permission.

## Route two: your distribution's package

Most distributions package Lynis
(`apt install lynis`, `dnf install lynis`,
and so on), and CISOfy also runs its own
repository with current packages. The
package route puts `lynis` on your PATH,
adds the man page, and gets updates with
the rest of the system. Its cost is
currency: a distribution package may lag
the upstream release by months, and a
security auditor whose checks predate a
year of new findings is quietly worth
less. Whichever route you choose, the
version check above — and the update
check in chapter 7 — tells you what you
actually have.

The lab did not take this route (it does
not install packages on the shared build
machine), so the guide does not show a
package install transcript; the unpack
above is the verified path, and the
package commands are the distribution's
own, documented in its package manager's
manual.

## First orientation

Two commands, both safe, both instant,
before any audit:

```bash
./lynis show version
./lynis show groups
```

The first confirms the version in the
program's own words. The second lists the
test groups an audit can cover —
`accounting`, `authentication`,
`kernel`, `ssh`, `firewalls`, and two
dozen more — which is the menu chapter 5's
report is drawn from. If `./lynis` refuses
to start at all, chapter 12 has the short
list of causes; on a healthy unpack there
is nothing to configure, and the next
chapter can start immediately.
