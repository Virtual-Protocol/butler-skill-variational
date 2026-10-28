# Reading Variational

Two reads, neither a card: they move nothing. Each is slow: run one once per errand,
never in a loop. Both are chat only.

## `acp site variational account [--market <SYMBOL>]`

Its summary and facts, as `acp site status <KEY>` shows them:

| Fact | Means |
| --- | --- |
| `Balance` | the account's balance, in USDC |
| `Unrealized PnL` | profit or loss across open positions, in USDC |
| `Withdrawable` | the most a withdrawal can ask for now |
| `Position <SYMBOL>` | one per open position: side and size in coins, entry, mark, value, PnL, liquidation (say "about") and how far below or above the mark it sits, margin (cross shares the account balance; isolated has its own), leverage, and that position's own take profit and stop loss |
| `Positions: none` | nothing open |
| `Market <SYMBOL>` | with `--market`: open for trading with its smallest long and short (coins, and dollars now); close-only; closed right now; not a market on Variational; or could not be checked |
| `Estimate <side> <size>` | with `--side` and `--usd`/`--size`: the open's size in coins, Variational's price, fee and liquidation for it, at the leverage and margin mode the market has now, and how much more that side takes ("Variational takes at most about $X more on this side now"; "THIS OPEN IS TOO BIG" when the open is above it). The open's card shows them; its liquidation and room only at that same leverage and mode. "not priced" says why (below the smallest order) |
| `Account: none yet` | Butler's wallet has no Variational account: a deposit or an open starts with the `signup` card |

Any figure can read `unknown`: say it is unknown, never zero. "no take profit" is none;
"take profit unknown" is not none. A liquidation's distance is from the mark when read:
the smaller it is, the closer the position is to being liquidated. "none given by
Variational": the read worked and Variational gave that position no liquidation price (so
far only at about 1x). Say exactly that; never call it safe or unknown.

## `acp site variational orders`

| Fact | Means |
| --- | --- |
| `Resting <id>` | an order waiting for its price: market, type, side, size, price, and `(order <id>)`, the id `cancel --order` takes |
| `Filled <id>` | a limit, take profit or stop loss that filled in the last day, and when |
| `Cancelled <id>` | one Variational cancelled in the last day, and why |
| `Orders: none ...` | nothing resting, nothing filled or cancelled in a day |

## When a read fails

`not_done`, `unknown` or a `SITE_*` code is **not** an empty account or order list. Say you
could not check it, and give the reason as written.
