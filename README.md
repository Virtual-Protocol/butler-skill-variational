# butler-skill-variational

Teaches a Butler agent to trade perpetuals on
[Variational Omni](https://omni.variational.io) for its owner, from Butler's own wallet:
read the balance, positions and resting orders, deposit, open a long or a short, close,
reverse, set isolated margin, place a limit order or a take profit and stop loss, cancel,
and withdraw. Any market Variational lists: crypto perps, swaps and stock perps.

The skill only instructs; the server does the work. Every act is `acp site variational …`,
which files an approval card; the owner approves it in the app, and the server carries it
out and posts how it ended. The reads are `acp site variational account` and `orders`.

One Butler skill, published by the
[Butler skill hub](https://github.com/Virtual-Protocol/butler-skills): the hub's
`skills.json` lists it by name, repo and ref.

- `SKILL.md` - the playbook (see the hub's
  [SKILL_STANDARD.md](https://github.com/Virtual-Protocol/butler-skills/blob/main/SKILL_STANDARD.md))
- `references/` - every command, reading the account and orders, orders that wait for a
  price, the sign-up card, errands that take more than one card, and what each `not_done`
  means
- `CHANGELOG.md` - one entry per version; every change bumps `version` in SKILL.md

No helper script: every step is a command the Butler sandbox already has.

Validate locally:

```sh
curl -sSLO https://virtual-protocol.github.io/butler-skills/tools/validate.py
python3 validate.py --standalone .
```
