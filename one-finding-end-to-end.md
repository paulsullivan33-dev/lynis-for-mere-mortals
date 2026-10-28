# One Finding, End to End

Outcome: one warning — FIRE-4512 — taken
from headline to understanding: what the
test checked, how to see its evidence,
how to confirm it by hand, what acting on
it would actually involve, and the
legitimate alternative of deciding it does
not apply. No setting on the lab machine
was changed; remediation is described,
not performed.

## The headline

```text
! iptables module(s) loaded, but no rules active [FIRE-4512]
```

In plain language: the kernel's packet
filter, **iptables**, is present and its
modules are loaded — the machinery of a
firewall exists — but there are no rules
in it. Every chain accepts everything. A
firewall that is switched on and empty is
not a weak firewall; it is no firewall,
wearing one's coat.

## The evidence, in the tool's own words

`lynis show details` prints the log lines
for one test:

```text
$ ./lynis show details FIRE-4512
2026-10-28 18:48:20 Performing test ID FIRE-4512 (Check iptables for empty ruleset)
2026-10-28 18:48:20 Result: iptables ruleset seems to be empty (found 0 rules)
2026-10-28 18:48:20 Warning: iptables module(s) loaded, but no rules active [test:FIRE-4512] [details:-] [solution:-]
```

(transcript t03). Three lines: what was
checked, what was measured — *found 0
rules* — and the judgement. Notice what
the test did not do: it did not guess. It
counted.

One trap lives here, and the lab fell into
it so you don't have to: put an option
*after* the test ID —
`lynis show details FIRE-4512 --no-colors`
— and the command prints nothing at all,
still exiting successfully. The ID must be
the last word. Silence from this command
means you asked wrongly, not that there
is nothing to see.

## Confirming it by hand

The finding is about `iptables`, so ask
`iptables` (transcript t04):

```text
$ iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

Three chains, no rules in any of them,
every policy ACCEPT. Lynis's count of zero
agrees with the tool's own listing. That
agreement is the point of the exercise:
a Lynis warning you have verified by hand
is knowledge; one you have not is a rumour
with a test ID. (The same transcript shows
the machine refusing `lsmod` — this
sandbox is a container without
`/proc/modules`. Verification uses the
tools that work, and says so.)

## What acting on it would involve

Fixing FIRE-4512 means giving the machine
a real ruleset: decide what may connect,
from where, and write rules that say so —
then make them survive a reboot. That is
a firewall guide's job, not a paragraph's
(the series' UFW guide teaches exactly
this work), and it is **not done in this
lab**, deliberately: this sandbox's
networking is a shared test rig whose
other tenants' work runs over the same
kernel, and an enthusiastic ruleset could
lock the machine — and the lab — out of
itself. The general risks are worth
naming, because they are why "just apply
the fix" is bad advice for this finding:

- A default-drop policy written before
  the allow rules can end your own access
  mid-sentence.
- Container and VM networking often
  *depends* on the NAT and forwarding
  rules other tools add to iptables;
  flushing first and asking later breaks
  them.
- Rules that are not persisted are a
  reboot away from never having existed.

Acting well means planning the ruleset on
a machine you can afford to lock, with
console access, in the firewall tool you
have chosen. Acting badly is one command
long. Lynis cannot tell the difference,
which is why it only reports.

## The other honourable outcome

Suppose you read all this and conclude:
this machine is a disposable sandbox on a
private test network; an empty ruleset is
a known, accepted property of it. Then
the correct action is not a fix but a
*decision* — recorded where future audits
will see it. That is a profile line,
`skip-test=FIRE-4512`, and chapter 7 adds
exactly that line to the lab's profile
and shows the warning leave the report —
with the log noting, for whoever reads it
later, that the test was *skipped by
profile setting*, not passed.
