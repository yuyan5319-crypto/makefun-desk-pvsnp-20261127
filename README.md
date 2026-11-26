# The P vs NP desk: a round's entry

This is the whole entry for a round of the P vs NP desk on make.fun: one file, `DESK.json`, naming the campaign the
entry joins, beside this README and its `LICENSE`. The entry carries no work. Your points come from the units you close
on the campaign's desk while the round is open, each one a Lean 4 file the campaign's validator checked against a
posted statement.

```json
{"schema": "makefun/desk-entry/v1", "campaign": "pvsnp"}
```

`DESK.json` holds exactly those two keys and nothing else. The tree may also hold a root licence file (this `LICENSE`),
a README, and make.fun's own files under `.makefun/`; any other file refuses the entry, since the work goes to the desk,
never the entry.

## Join

    makefun desk join --campaign pvsnp

`join` sends this one-file entry through make.fun's ordinary submit path as a source bundle, and such an entry is
always signed by a Solana wallet linked to your account, whether or not the round pays. It signs with
`--keypair <file>` or `--signer-cmd <cmd>` when you name one, else with the signer `makefun login` stored. An
account that has not joined the open round is entered by `makefun desk next` first; `--no-join` leaves that out and
prints a reminder.

## Work on the desk

A file reaches the desk one of two ways: as a pull request on the campaign's public repository, or sent directly from
make.fun's command line. The desk checks both the same way, and both count in the round once your account has joined
it. The first valid file by receipt takes the unit. A later valid file for a unit already taken is recorded as a
duplicate and scores nothing. A file the validator refuses scores nothing and counts as a refused check, the board's
tie-break.

The desk posts two kinds of unit: ordinary units, one contributor to a unit, and hard units, the heavier statements,
which several contributors work on at once, each on a seat of their own. `makefun desk next --campaign pvsnp` leases
you an ordinary unit and `makefun desk next --campaign pvsnp --hard` a hard one. An account holds only so many leases at
once, and a new account fewer, so when the kind you ask for answers `lease_cap`, release the lease you hold with
`makefun desk release <lease> --reason <reason>` before you ask again. A unit you release with any reason but
`submitted` is not leased to you again for an hour.

### By pull request

The campaign's public repository is https://github.com/makedotfun/pvsnp. The desk reads the pull requests opened on it
and checks the file each one adds.

1. Link the GitHub account you will open the pull request from to your make.fun account, on your profile. The desk
   knows whose pull request it is reading by that link. A pull request from a GitHub account that no make.fun account
   links is not checked until the link is made, and one still unlinked after a while is closed.
2. Join the round first, as Join above says. A pull request does not join the round for you.
3. Pick a free unit on the campaign's page on make.fun (`/desk/pvsnp`), which shows each unit's key, statement and
   weight. Your pull request takes the unit while it is free and your account has room for one more lease; on a hard
   unit it takes one of the unit's free seats. A unit `makefun desk next --campaign pvsnp` or `makefun desk next
   --campaign pvsnp --hard` leased you works too: its card names the repository and the exact file a pull request for
   it adds, and `makefun desk check` lints that file on your own machine before you open it.
4. Fork the repository, make a branch from the current `main` and add exactly one file, `PvsNP/Units/<key>.lean` (a
   dash in the unit's key is written as an underscore). Change nothing else.
5. Open the pull request against `main`. One unit to a pull request.

The desk reads open pull requests at intervals of a minute or more and keeps one status comment on each, edited as the
check goes on: checked and merged, refused with its reason, or waiting and what it waits on. A draft pull request is not
read until it is marked ready. If yours is refused, push a fix to the same branch while your lease lasts: the new head
is checked as a new attempt.

A pull request is public from the moment you open it. Its file is received when the desk first reads the commit it
checks, not when the pull request was opened, and a fix you push later is received when the desk reads it. The first
valid file for a unit is merged by the desk. A later valid file for the same unit is a duplicate, and its pull request
is closed.

### Directly, from the command line

This way needs no fork and no pull request, and your file stays private until its verdict.

1. `makefun desk next --campaign pvsnp` leases you one ordinary unit, and `makefun desk next --campaign pvsnp --hard`
   one hard unit, and each prints its card: the statement, its weight, its lane, its negated twin and, for a hard unit,
   how many seats it has. The lease is your seat on that unit while you work; renew it with `makefun desk beat <lease>`
   (`--watch` renews it for you) and give it back with `makefun desk release <lease> --reason <reason>` when you stop.
2. Write the Lean file that proves the statement, in the campaign's definitions (`PvsNP.Defs`), or that proves its
   negated twin: a refutation scores the same.
3. `makefun desk check <file> --unit <id>` runs the validator's lint on your own machine. It is free and starts no
   check, so run it until it passes.
4. `makefun desk submit <file> --lease <lease>` sends the file. The desk checks it in a sandbox with no network and
   replays it through independent Lean kernels; that takes minutes, not seconds. Measured once on make.fun, on one
   proof, on 2026-11-25: about 10 minutes of checking for a small proof once the machine holds the checker, up to about
   20 the first time, longer for a heavier proof, and a check waits its turn while another check runs. `makefun desk
   status <submission> --wait` follows it.

## What a credit says

A credit means the file was checked by the Lean kernels against the statement as it was posted. It does not mean a
result about P vs NP is proved: the definitions the statements are written in are not yet human-audited.

## The limits

How many checks an account may start on the campaign is limited by account reputation: each accepted check grows the
allowance and each refused check shrinks it for a while, while a lint never counts and a check that faults is given
back. A new account holds fewer leases at once.

Your make.fun account needs a GitHub account linked: an accepted file is published in the campaign's public repository
under the contributor's GitHub identity.
