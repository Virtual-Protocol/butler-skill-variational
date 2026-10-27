# Reading Variational

Two reads, neither a card: they move nothing. Each opens a browser on the server: run one
once per errand, never in a loop. Both are chat only.

## `acp site variational account`

Its summary and facts, as `acp site status <KEY>` shows them:

| Fact | Means |
| --- | --- |
| `Balance` | the account's balance, in USDC |
| `Unrealized PnL` | profit or loss across open positions, in USDC |
| `Withdrawable` | the most a withdrawal can ask for now |
| `Position <SYMBOL>` | one per open position: side and size in coins, entry, mark, value, PnL, liquidation (say "about"), margin mode and leverage |
| `Positions: none` | nothing open |
| `Account: none yet` | Butler's wallet has no Variational account: a deposit or an open starts with the `signup` card |

Any figure can read `unknown`: say it is unknown, never zero.

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
