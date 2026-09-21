# ARBITER


A smart-contract auditor whose findings are transcripts of executions rather than
assertions. The agent cannot report a vulnerability; it has to write an attack that
the harness compiles and runs, against a success condition the agent never sees.

Everything is under [`arbiter/`](arbiter/). Start with
[`arbiter/README.md`](arbiter/README.md).


> **Fork note** — This is a fork of [yeee3642/AIS3_CTF1](https://github.com/yeee3642/AIS3_CTF1)
> (**ARBITER**), a team project from AIS3 新型態資安暑期課程 2026, where it was selected as **Best Project**.
> I keep this fork as part of my portfolio; full authorship and history live in the original repo.
>
> **My role (4-person team, ~25%):** presentation lead; reviewed and challenged the overall
> system architecture and proposed improvements; contributed code fixes.

## The evidence ladder

A finding here is not one claim. It is a claim that survived a particular amount of
scepticism, and `arbiter prove` reports how much:

| rung | what it means | what we gave up |
|---|---|---|
| 1 synthetic | the harness accepted it: its own predicate, a negation test, six gates | nothing yet -- the harness is still the EVM's administrator |
| 2 standalone | the dumped project compiles and passes under `forge test` elsewhere | nobody has to trust our summary |
| 3 live | reproduced as signed transactions on a chain | the administrator: no cheatcodes, real keys, real gas |

The rungs are not redundant. Rung 1 is the only one that searches; rung 3 cannot find
anything at all, it can only refuse. On the twelve exploits dumped from the last run it
refused both false positives that rungs 1 and 2 had accepted, and refused them on their
merits rather than by failing to run.

## Quick look

```bash
cd arbiter
python3 scripts/victim_probe.py      # the gates, proved in both directions, no network
python3 scripts/live_attack.py       # a reentrancy drained on a local chain
python3 scripts/live_attack.py --patched   # one line moved; the attack reverts
```

## What was here before

This repository also held `bastet-cc`, a separate project. It was removed in the commit
tagged `pre-arbiter-prune`, which is the commit to check out to get it back:

```bash
git checkout pre-arbiter-prune -- bastet-cc
```
