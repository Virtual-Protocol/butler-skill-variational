# Orders that wait for a price

A limit order and a TP/SL rest on Variational and fill later, when no one is watching. So
their card is done once the order **rests**: say "placed", never "filled". Nobody is told
when one fills: `acp site variational orders` lists the resting ones, and those that filled
or were cancelled in the last day. Read it when your owner asks how an order stands.

Each is filed with its line in `SKILL.md` step 3; this file is what the flags do.

## Limit (`limit`)

- `--side` is `long` or `short`. In coins, `--size <COINS>` instead of `--usd`. Add
  `--margin isolated` only when they ask for isolated, and `--reduce-only` for a close at a
  price.
- It fills in full or not at all, at the limit price or better, and expires after 120 days.
- Dollars are sized at the limit price, not the price now.
- A limit that would fill at once (a buy at or above the ask, a sell at or below the bid)
  is refused with nothing sent. That is an open: use `open`.
- `--reduce-only` makes it a close at a price: it needs a position on the other side, no
  larger than it.
- Leverage and margin are set when it is placed, as for an open.

## Take profit and stop loss (`tpsl`)

- On a position they hold. Either one alone (drop `--tp` or `--sl`), or both.
- It replaces the take profit and stop loss already there: the old ones are cancelled
  first, or nothing new is placed.
- Each fires as a market order when the mark price reaches it, and closes the whole
  position; it resizes with the position.
- A trigger on the wrong side of the mark (a long's stop above the mark) is refused, since
  it would fire at once.
- On a new position, put them on the open instead: add `--tp <PRICE>` and `--sl <PRICE>` to
  its `open`.

## Cancel (`cancel`)

Add `--order <ID>` to cancel one order. Without it, it cancels every resting order on that
market. The id is the `(order ...)` at the end of its line in `acp site variational orders`. Each cancel is confirmed by
Variational before the card counts it done.
