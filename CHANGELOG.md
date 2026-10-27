# Changelog

## 2.0.3

- Deposits and withdrawals pass `--fee`, Variational's `Transfer fee` from this errand's
  account read; the card shows it and the run never lets Variational take more. Tell the
  owner the fee before filing. A fee read as `unknown` means no deposit or withdrawal yet.
- A gap the size of the card's fee after a deposit or withdrawal is that fee, never missing
  money.
- Never write a Platforms row for Variational: nothing keeps it current.
- Needs the server that requires `--fee` (deposits and withdrawals without it are refused
  there; with it, an older server refuses the unknown flag).

## 2.0.2

- Reads (`account`, `orders`) take no idempotency key. The server answers a key it has
  seen in the last 7 days with its first result, so a reused key on a read gave an old
  balance that looked live.
- Every card's key is made from the errand: market, side, size and the date and time of
  the ask (`var:eth:long:50usd:20261027-1244`), plus the step (`:open`, `:deposit`). The
  old `var:eth-long:open` repeated, so the same ask next week was told "already filed".

## 2.0.1

- The description names the reads too (balance, positions, orders, read live every time),
  so a "what's my balance" ask picks this skill instead of answering from memory.
- Figures are never taken from memory or an earlier chat, and never saved to memory.
- No internal names in the skill: no repositories, and the platform is "the server".
- Say to the owner: a read in plain words, and never the browser, the server, an
  idempotency key or an approval id.
- The description is a quoted string, so it can hold a colon.

## 2.0.0

- On the server's site rail: every act is `acp site variational <action>`, the reads are
  `acp site variational account` and `orders`, and a card is followed with
  `acp site status <key>`. The rail's own output and codes say what to do; this skill adds
  only what is Variational's.
- Sign-up is its own card (`signup`); the owner types the access code on it.
- A deposit the wallet can't cover ends `not_done` after approval: read the wallet first.
- No pushed fill notes: `orders` lists resting orders and those that filled or were
  cancelled in a day.
- `references/flows.md`: errands that take more than one card.

## 1.0.1

- A deposit the wallet can't cover is refused before any card
  (`VARIATIONAL_INSUFFICIENT_USDC`): read every chain's USDC before offering a bridge, and
  never offer more than a chain holds.
- The account read is relayed in plain words, never field names: a wallet with no account
  answers `hasAccount: false` with a `hint` to say instead.

## 1.0.0

- First version. The playbook that was six rows in the built-in `acp` skill, as a hub
  skill: read the account and resting orders with `bevo-read get /variational/*`, then file
  each act with `acp variational …` and follow its card with
  `bevo-read request <key> --route variational`.
- Every command in `references/commands.md`.
- The first card on a new account comes back for the access code; the owner enters it on
  the card (`references/setup.md`).
