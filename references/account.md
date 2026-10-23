# Reading Variational

Two reads, neither a card: they move nothing. Each opens a browser on bevo-server, so each
answers at most once a minute (`VARIATIONAL_READ_BUSY` otherwise). Wait, never loop. Both
are refused from a duty.

## `bevo-read get /variational/account`

```json
{ "status": "ok", "hasAccount": true, "balance": "52.10", "upnl": "1.84", "withdrawable": "20.00", "positions": [ ... ] }
```

| Field | Means |
| --- | --- |
| `hasAccount` | `false` when Butler's wallet has no Variational account yet; nothing else comes with it |
| `balance` | the account's balance in USDC, as Variational's portfolio reports it |
| `upnl` | unrealized profit or loss across open positions, in USDC |
| `withdrawable` | the most a withdrawal can ask for now |
| `positions[]` | each open position, below |

### A position

| Field | Means |
| --- | --- |
| `market`, `type` | the symbol, and its kind: `perpetual_future` (crypto), `perpetual_rwa_future` (a stock perp) or `swap` |
| `side`, `size` | `long` / `short`, and the size in coins |
| `entryPrice`, `markPrice` | the average entry, and the mark now |
| `value` | the position's value at the mark |
| `upnl` | its unrealized profit or loss |
| `liquidationPrice` | where Variational liquidates it; `null` when it has none to report. Say "about" |
| `margin` | `cross` or `isolated`; `isolatedMargin` is an isolated position's own margin |
| `leverage` | the market's leverage; `null` when it could not be read |

Relay the figures as given. Any of the prices, `value`, `upnl` or `leverage` can be `null`:
say it is unknown, never zero.
The read also looks at resting orders, so a fill since the last look is told in chat.

## `bevo-read get /variational/orders`

```json
{ "status": "ok", "checked": true, "orders": [ { "id": "...", "market": "ETH", "type": "limit", "side": "buy", "price": "2000", "qty": "0.025", "status": "pending" } ] }
```

Only orders Butler placed. `checked: false` with no orders means none are resting, and no
browser opened. Each order has the `id` that `cancel --order` takes, its `type` (`limit`,
`take_profit`, `stop_loss`), `side`, `price`, `qty` (`null` for a TP/SL, which covers the
whole position) and `status`: `pending`, `filled`, `canceled`, or `unknown` when
Variational did not say.

## When a read fails

A non-zero exit, `VARIATIONAL_READ_BUSY` or `VARIATIONAL_READ_FAILED` is **not** an empty
account or order list. Say you could not check it, and give the server's `hint`.
