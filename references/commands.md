# Every Variational command

Each one files an approval card; nothing runs until the owner approves it. The rail
reads the flags strictly: an unknown flag, a missing one or a value it can't read is
`SITE_BAD_ARGS` and nothing is filed. Amounts are positive, at most 6 decimals.
Add `--idempotency-key <KEY>:<step>` to every one, with this errand's key (SKILL.md
§ Idempotency and retries). The reads `account` and `orders` never take one.

| Command | What the card does |
| --- | --- |
| `acp site variational signup` | Sets up Butler's wallet on Variational; your owner types the access code on the card (`references/setup.md`) |
| `acp site variational deposit --amount <usdc>` | Deposits that much USDC from Butler's wallet on Arbitrum |
| `acp site variational open --market <SYMBOL> --usd <n> [--leverage <1-5>] [--side long\|short]` | Opens (or adds to) a position worth `<n>` dollars at the price |
| `acp site variational open --market <SYMBOL> --size <coins> [--leverage <1-5>] [--side long\|short]` | The same, sized in coins ("long 2 ETH" is `--size 2`) |
| `acp site variational close --market <SYMBOL>` | Closes the whole position |
| `acp site variational close --market <SYMBOL> --percent <1-100>` | Closes that share of it |
| `acp site variational close --market <SYMBOL> --size <coins>` | Closes that many coins of it |
| `acp site variational reverse --market <SYMBOL>` | Flips a long into a short of the same size, or the reverse |
| `acp site variational margin --market <SYMBOL> --amount <usdc>` | Sets an isolated position's margin to that amount |
| `acp site variational limit --market <SYMBOL> --side long\|short (--usd <n> \| --size <coins>) --price <p> [--leverage <1-5>] [--margin cross\|isolated] [--reduce-only]` | Places an order that waits for its price (`references/resting-orders.md`) |
| `acp site variational tpsl --market <SYMBOL> [--tp <price>] [--sl <price>]` | Sets or replaces the take profit and stop loss on a held position |
| `acp site variational cancel --market <SYMBOL> [--order <id>]` | Cancels every resting order on that market, or one |
| `acp site variational withdraw --amount <usdc>` | Withdraws that much USDC to Butler's wallet |

An `open` also takes:

- `--margin cross|isolated` - only for a new position; cross unless they ask. A market
  Variational keeps isolated (swaps) is isolated anyway. A position keeps its mode: asking
  for the other one is refused with nothing sent.
- `--tp <price>` and `--sl <price>` - a take profit or stop loss on the new position.

## How each one behaves

- **open** takes exactly one of `--usd` and `--size`. A long buys at the ask, a short sells
  at the bid. It only goes the way a position already points: a short while a long is open
  is refused with nothing sent, because that would be a close.
- **Leverage** is 1x when left out. An open sets it for the whole market, so it re-levers a
  position already there. A close or reverse never changes it.
- **close** is reduce-only, so it can never flip a position. A part close is rounded down
  to Variational's tick; below its minimum, or more than the position, is refused with
  nothing sent.
- **reverse** is one order for twice the position on the other side. It counts as done only
  when the position is the exact mirror of before.
- **margin** needs an isolated position, and Variational refuses less than the position
  needs. Closing the last of an isolated position returns its margin to the account.
- **deposit** ends `not_done` with nothing moved when Butler's wallet holds less USDC on
  Arbitrum than the amount: read the wallet before filing (`SKILL.md` step 2).
- **withdraw** goes only to Butler's wallet, less Variational's fee. It stays `running`
  until the USDC lands in the wallet, usually within minutes.

There is no dollar cap of Butler's own: the owner's approval is the cap, and the bounds are
Variational's (its order sizes, what can be withdrawn) and the wallet's.
