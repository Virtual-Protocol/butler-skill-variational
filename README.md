# butler-skill-variational

Teaches a Butler agent to trade perpetuals on
[Variational Omni](https://omni.variational.io) for its owner, from Butler's own wallet:
read the balance, positions and resting orders, deposit, open a long or a short, close,
reverse, set isolated margin, place a limit order or a take profit and stop loss, cancel,
and withdraw. Any market Variational lists: crypto perps, swaps and stock perps.

Two things shape how it works:

- **The skill only instructs; bevo-server does the work.** Variational has no public
  trading API, and its session can trade and withdraw with no signature, so no Butler
  sandbox ever holds it. Every act is `acp variational …`, which files an approval card;
  the owner approves it in the app, and bevo-server's runner carries it out in a browser of
  its own and posts how it ended. The reads are `bevo-read get /variational/account` and
  `/variational/orders`. bevo-server `docs/variational.md` is the reference for both.
- **Only for owners Variational is turned on for.** Everyone else gets
  `VARIATIONAL_NOT_ENABLED`, which the skill relays. It is chat only: a duty is refused.

One Butler skill, published by the
[Butler skill hub](https://github.com/Virtual-Protocol/butler-skills): the hub's
`skills.json` lists it by name, repo and ref.

- `SKILL.md` - the playbook (see the hub's
  [SKILL_STANDARD.md](https://github.com/Virtual-Protocol/butler-skills/blob/main/SKILL_STANDARD.md))
- `references/` - every command, reading the account and orders, orders that wait for a
  price, and the first card that sets up the account
- `CHANGELOG.md` - one entry per version; every change bumps `version` in SKILL.md

No helper script: every step is a command the Butler sandbox already has.

Validate locally:

```sh
curl -sSLO https://virtual-protocol.github.io/butler-skills/tools/validate.py
python3 validate.py --standalone .
```
