# Errands that take more than one card

One card at a time: say the count first, file the next only once the last is `done`, and
stop at the first that is not. A key per step, from one errand
name: `var:eth-long:deposit`, `var:eth-long:open`.

## First trade, no Variational account yet

"Long ETH on Variational, $150 at 3x" on a wallet with no account:

1. `account`: no account yet. The trade needs about $50 of margin ($150 at 3x).
2. Check Butler's USDC on Arbitrum (`SKILL.md` step 2). Short: offer a bridge, and only
   once they agree.
3. `signup`: your owner types the access code on its card. Wait for `done`.
4. `deposit`. Wait for `done`.
5. `account` again, then `open`. Three cards in all: say so before the first.

## Fund and trade, account already there

1. `account`. Free balance enough for the margin (size / leverage)? Skip to step 3.
2. Not enough: `deposit` the difference, as above, and wait for it.
3. `open`. A stop or a target they named goes on the open itself (`--sl`, `--tp`): one card.

## Close and take the money out

1. `account`: the position and its size.
2. `close`. Wait for `done`.
3. `account` again: withdraw at most `Withdrawable` as it is now, not before the close.
4. `withdraw`. It stays `running` until the USDC lands, usually minutes: say so.

## Flip, with a new stop

1. `reverse`. Wait for `done`.
2. Only if they asked: `tpsl` for the new side (a stop on a short sits above the mark).

## Buy at a price, then protect it

1. `limit`. Done means placed, not filled.
2. A `tpsl` needs a position, so it waits for the fill: `orders` shows it once it fills.
   Offer the TP/SL then; never file it ahead.

## Everything off Variational

1. `account`. One `close` per open position, one at a time.
2. `cancel` each market with resting orders (`orders` lists them).
3. `account` again, then one `withdraw` of `Withdrawable`.
