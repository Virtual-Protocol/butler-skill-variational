# Changelog

## 2.0.5

- Before an open, or a deposit meant for one, read `account --market <SYMBOL>`. Closed,
  close-only or not listed: say so and file nothing.
- The minimum is the `Market` line's smallest order, never Hyperliquid's $15 perp minimum.
- For an open, the read also takes its `--side` and `--usd`/`--size`. Its `Estimate` line is
  Variational's price, fee and liquidation for that open, and the open's card shows the same.
  An open whose live liquidation estimate is much closer than its card's ends `not_done` with
  nothing sent.
- Liquidation direction comes only from the read ("13% below the mark") or the margin
  card's result ("closer"), never from comparing two liquidation prices yourself.
- Before adding isolated margin, name any cross position: its liquidation moves closer.
- Each position's TP/SL is on its own line; never lend one position's to another.
- The server now writes the Variational Platforms row; still never write one yourself.
- Needs the server whose account read takes `--market`, `--side` and `--usd`/`--size`, and
  shows each position's TP/SL and liquidation distance. An older server refuses those flags
  as unknown.

## 2.0.4

- A wrong access code: tell the owner and wait. File a new `signup` card only when they ask
  to try another code, never on your own.

## 2.0.3

- Deposits and withdrawals pass `--fee`, Variational's `Transfer fee` from this errand's
  account read; the card shows it and the run never lets Variational take more. Tell the
  owner the fee before filing. A fee read as `unknown` means no deposit or withdrawal yet.
- A gap the size of the card's fee after a deposit or withdrawal is that fee, never missing
  money.
- Never write a Platforms row for Variational: nothing keeps it current.
- Needs the server that requires `--fee`: it refuses a deposit or withdrawal without one,
  and an older server refuses the flag as unknown.

## 2.0.2

- Reads (`account`, `orders`) take no idempotency key. The server answers a key it has seen
  in the last 7 days with its first result, so a read with a reused key is not live.
- Every card's key is made from the errand: market, side, size and the date and time of
  the ask (`var:eth:long:50usd:20261027-1244`), plus the step (`:open`, `:deposit`). The
  same ask at another time gets a new card, not "already filed".

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
  skill: read the account and resting orders through the server's read route, then file
  each act with `acp variational …` and follow its card through the server.
- Every command in `references/commands.md`.
- The first card on a new account comes back for the access code; the owner enters it on
  the card (`references/setup.md`).
