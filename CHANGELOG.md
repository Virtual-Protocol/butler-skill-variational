# Changelog

## 2.0.9

- An owner with no access code is told where one comes from: someone who uses Variational,
  or Variational's Discord, https://discord.gg/variational (from Variational's official
  links page). Never a code, a code-listing site or a referral link.
- No server change needed: the sign-up card still only says to type the code, so Butler is
  the one who says where to get it.

## 2.0.8

- "Which swaps are open or closed?" is one `acp site variational markets` read, in seconds
  and with no sign-in, never one `account --market` read per market. It can be up to 10
  minutes behind; before filing on a market, its `account --market` line is still the answer.
- Needs the server with the `markets` read; an older one does not know it.

## 2.0.7

- "How much more can I add?" comes from the `Market` line, which now says how much more
  each side takes. Never price an order the owner did not ask for to find it, and never
  say how a figure was read.
- Price an open with its `--leverage` too: a room is only for the leverage it names (an
  unused market sits at Variational's default, 20x on NVDA, where far more fits than at 1x).
  Never offer a room at another leverage.
- Needs the server whose `Market` line carries each side's room and whose read takes
  `--leverage`; an older one says "how much more it takes is unknown" or refuses the flag.

## 2.0.6

- The `Estimate` line also says how much more Variational takes on that side now. An open
  above it ("THIS OPEN IS TOO BIG") is not filed: Variational would take it and then turn it
  down on a risk check. Never judge that from withdrawable or balance: the leverage limit is
  not in them.
- A turned-down order now ends `not_done` with Variational's reason ("took the order, then
  failed …", "refused the order: …"): read again and offer what fits.
- Variational can't be reached: nothing was sent; try once more later.
- Margin: `--amount` is the new total to the cent (the read's margin plus the change),
  and after it say only what the result says about each liquidation.
- "none given by Variational" is a position the read gave no liquidation price for; say
  that, never "unknown" or "safe".
- The failure table moved to `references/failures.md` (unchanged rows, plus the two above),
  keeping SKILL.md under the hub's 12,000-character body limit.
- Needs a server version whose estimate shows the side's room and whose results give
  Variational's reason. With an older one the skill still works, without the check.

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
- Needs a server version whose account read supports `--market`, `--side` and
  `--usd`/`--size`.

## 2.0.4

- A wrong access code: tell the owner and wait. File a new `signup` card only when they ask
  to try another code, never on your own.

## 2.0.3

- Deposits and withdrawals pass `--fee`, Variational's `Transfer fee` from this errand's
  account read, and the card shows it. Tell the
  owner the fee before filing. A fee read as `unknown` means no deposit or withdrawal yet.
- A gap the size of the card's fee after a deposit or withdrawal is that fee, never missing
  money.
- Never write a Platforms row for Variational: nothing keeps it current.
- Needs a server version that supports `--fee`.

## 2.0.2

- Reads (`account`, `orders`) take no idempotency key, so every read is live.
- Every card's key is made from the errand: market, side, size and the date and time of
  the ask (`var:eth:long:50usd:20261027-1244`), plus the step (`:open`, `:deposit`). The
  same ask at another time gets a new card, not "already filed".

## 2.0.1

- The description names the reads too (balance, positions, orders, read live every time),
  so a "what's my balance" ask picks this skill instead of answering from memory.
- Figures are never taken from memory or an earlier chat, and never saved to memory.
- No internal names in the skill: no repositories, and the platform is "the server".
- Say to the owner: a read in plain words, and never internal terms such as an
  idempotency key or an approval id.
- The description is a quoted string, so it can hold a colon.

## 2.0.0

- Every act is `acp site variational <action>`, the reads are
  `acp site variational account` and `orders`, and a card is followed with
  `acp site status <key>`. The command's own output and codes say what to do; this skill
  adds only what is Variational's.
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
  skill: read the account and resting orders, then file each act with
  `acp variational …` and follow its card.
- Every command in `references/commands.md`.
- The first card on a new account comes back for the access code; the owner enters it on
  the card (`references/setup.md`).
