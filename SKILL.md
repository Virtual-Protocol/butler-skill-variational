---
name: variational
description: "Look up or trade on Variational Omni from Butler's wallet: balance, positions and orders (read live, never from memory), deposit, open, close, limit, TP/SL, cancel, withdraw, each act a card."
version: 2.0.9
metadata: {"butler":{"moneyMoving":true,"keywords":["variational","variational omni","omni.variational.io","trade on variational","long on variational","short on variational","variational position","close my variational","variational balance","variational account","variational orders","deposit to variational","withdraw from variational","variational limit order","variational take profit","variational stop loss","cancel variational order"],"requires":{"bins":["acp","bevo-read"]}}}
---

## When to use

Your owner names **Variational** (Omni, omni.variational.io) and wants to:

- **Look** - "my Variational balance", "my positions there", "my orders": step 1, then stop.
- **Which markets are open** - "which swaps on Variational are closed now?", "is gold
  trading?": `acp site variational markets`, then stop (`references/account.md`).
- **Fund** - "put 20 USDC into Variational": steps 1 to 4 with `deposit`.
- **Open** - "long ETH on Variational with $50 at 3x": steps 1 to 4.
- **Close, reverse, re-margin** - "close half my SOL", "flip it", "set my margin to 30": steps 1, 3, 4.
- **Wait for a price** - "buy ETH at 2,000", "stop loss at 2,300": steps 1, 3, 4, and
  `references/resting-orders.md`.
- **Withdraw** - "take my money out of Variational": steps 1, 3, 4 with `withdraw`.
- **More than one act** - "fund it and long ETH", "close and withdraw": `references/flows.md`.

Not this skill: Hyperliquid perps (`acp trade`), Papertrade, or anything your owner did
not say is on Variational. Every Variational act and read is `acp site variational …`;
this skill says which action fits an ask and what Variational's answers mean.

## Before you start

1. **Variational is its own venue.** Its balance and positions are only in its own reads;
   Butler's wallet balance and Hyperliquid positions are something else.
2. **Figures come only from this turn's reads and the card**, never from memory or an
   earlier chat, and never save one to memory. An unreadable account is not an empty one.
   Never work out a liquidation price, a fee or a PnL yourself, and never compare two
   liquidation prices: whether one came closer is only what the read or the card says
   ("13% below the mark", "closer"). A long's liquidation price rising is it coming closer.
3. **Leverage is 1 to 5, and 1 unless they name one.** An open sets the leverage for the
   whole market, so it re-levers a position already open there: say so.
4. **The access code is never in chat.** Your owner types it on the `signup` card
   (`references/setup.md`). Never ask for it, never repeat one they paste, and never put
   it in a command.
5. **From chat only.** A duty is refused (`SITE_DUTY_NOT_ALLOWED`).
6. **You never touch Omni's page.** No browser, no wallet connection, no clicks: the server
   runs every step.
7. **Never write a Platforms row for Variational** (`bevo-platforms set`): it is kept
   current for you.
8. **The minimum is Variational's, never Hyperliquid's.** The $15 perp minimum you know is
   Hyperliquid's. Variational's smallest order is per market and side: quote only the
   `Market <SYMBOL>` line of this errand's read, and never offer a size below it.

### Never file an open without all three

1. **They chose it:** the market, long or short, the size (dollars or coins) and the
   leverage, in their own words. "You pick" is not a choice: ask.
2. **They know the bad case:** at higher leverage a smaller move against them takes the
   whole margin. The card shows the figures; tell them to read it before they approve.
3. **The read is fresh:** step 1 ran after their last answer, so you know whether they
   already hold that market and on which side.

## Procedure

1. [FIXED] Read the account, before any act and whenever asked:

   ```sh
   acp site variational account
   acp site variational account --market <SYMBOL>
   acp site variational account --market <SYMBOL> --side <SIDE> --usd <USD> --leverage <N>
   ```

   Add `--market <SYMBOL>` whenever the errand will open or place an order there, including
   a deposit meant for that trade. Its `Market <SYMBOL>` line says whether the market trades
   now and its smallest order each way. **Closed, close-only (for an open) or not listed:
   say so and file nothing, not even the deposit.** Stock perps and swaps keep market hours.
   To say which swaps are open or closed, run `acp site variational markets` once: never
   one `--market` read per market to find out.

   Before an `open`, also give the open's `--side`, `--usd` (or `--size`) and `--leverage`, exactly as you
   will file it. The `Estimate` line is Variational's price, fee and liquidation for it, and
   the open's card shows the same if it is filed within 10 minutes with the same market,
   side and size. Quote those figures only from that line; another size gets none.

   For resting orders, and those that filled or were cancelled in the last day:
   `acp site variational orders`. Never give a read a key: a key seen before answers with
   its first, stale result instead of reading again. Relay what it says in plain
   words (`references/account.md`). "Butler's wallet has no Variational account yet":
   say so, and that a deposit or an open starts with a sign-up card. A read is slow:
   run it once per errand, never in a loop. `not_done` or `unknown`: say you
   could not check it. Its `Transfer fee` is Variational's fee on every deposit and
   withdrawal: keep it for step 3's `--fee`.

2. [ADAPT] Match the ask to the position you just read:
   - **Open on a market they hold on the same side**: it adds to that position, and the
     leverage applies to all of it. Say so.
   - **Open on the other side of a position they hold**: offer `close` or `reverse`.
   - **Open or limit size**: at least the `Market` line's smallest order for that side. Below
     it, say the smallest and ask; never pick a size for them.
   - **Open size, the top**: the `Market` line says how much more each side takes now; the
     `Estimate` line says it for the open. If it ends "THIS OPEN IS TOO BIG", file nothing:
     say how much it takes and ask what they want (that size or less, a deposit first, or
     nothing). "How much more can I add?" is the `Market` line: never price an order they
     did not ask for, and never say how a figure was read. A room is only for the leverage it
     names: never offer one at another leverage. Never judge it from withdrawable or balance.
   - **Close, reverse, margin, TP/SL**: only on a market they hold. `margin` needs an
     isolated position.
   - **More margin on an isolated position**: `--amount` is the new total, not the change:
     the read's margin plus what they add, to the cent ($0.965354 + $1 → `1.97`). It comes
     out of the cross balance: name any cross position and say its liquidation moves
     closer. After it, say only what the result says about each liquidation.
   - **TP/SL**: each position's own line says its take profit and stop loss. Never carry one
     position's to another; "unknown" is not "none".
   - **Deposit**: Butler's wallet must hold the USDC on Arbitrum. Check first with
     `bevo-read get /user-assets --param token=USDC --param fresh=1`. If it holds less,
     offer a bridge only from a chain that holds enough, never more than it holds; if
     none does, say how much there is.
   - **Withdraw**: at most what the read showed as withdrawable.
   - **Deposit or withdraw, the fee**: say it before filing, from the read's `Transfer fee`:
     depositing X adds X less the fee, withdrawing X sends X less the fee. If the read says
     the fee is `unknown`, don't file: say you could not check the fee, and read again later.

3. [FIXED] File the card, once, with this errand's key (§ Idempotency and retries;
   `references/commands.md` has every form):

   ```sh
   acp site variational open --market <SYMBOL> --usd <USD> --leverage <N> --side <SIDE> --idempotency-key <KEY>:open
   acp site variational close --market <SYMBOL> --idempotency-key <KEY>:close
   acp site variational reverse --market <SYMBOL> --idempotency-key <KEY>:reverse
   acp site variational margin --market <SYMBOL> --amount <USDC> --idempotency-key <KEY>:margin
   acp site variational limit --market <SYMBOL> --side <SIDE> --usd <USD> --price <PRICE> --leverage <N> --idempotency-key <KEY>:limit
   acp site variational tpsl --market <SYMBOL> --tp <PRICE> --sl <PRICE> --idempotency-key <KEY>:tpsl
   acp site variational cancel --market <SYMBOL> --idempotency-key <KEY>:cancel
   acp site variational deposit --amount <USDC> --fee <FEE> --idempotency-key <KEY>:deposit
   acp site variational withdraw --amount <USDC> --fee <FEE> --idempotency-key <KEY>:withdraw
   ```

   `<FEE>` is this errand's `Transfer fee` from step 1, as it was read (`0.1`). The card
   shows it, and the run never lets Variational take more.

   `<SYMBOL>` is what your owner named (`ETH`, `SOL`, `XAUS`, `AAPL`). `<SIDE>` is `long`
   or `short`. An open or limit sized in coins takes `--size <COINS>` instead of `--usd`;
   a part close adds `--percent <1-100>` or `--size <COINS>`. The reply is a card in this
   chat: tell your owner what it does, and to approve it in the app.

4. [FIXED] Follow it; never file it again to find out: `acp site status <KEY>:<STEP>`.
   - `done` - relay the summary and facts as written. A limit or TP/SL is "placed", not
     filled.
   - `running` - approved and running. A withdrawal stays here until the USDC lands in
     Butler's wallet, usually minutes. The chat is told when it ends.
   - `not_done` saying Butler's wallet has no Variational account - file
     `acp site variational signup --idempotency-key <KEY>:signup`; your owner types the
     access code on that card. Once it is `done`, file the original ask again with a new
     errand key (a new time in it).
   - `unknown` - it may or may not have happened. Read step 1 before filing anything.
   - `rejected`, `failed` or a `SITE_*` code - relay what `acp` printed; it says what to do.

## Idempotency and retries

Reads (`account`, `orders`) never take a key. Every card does: make one errand key from
what was asked and when, such as `var:eth:long:50usd:20261027-1244` (market, side, size,
the date and time of the ask), and add the step: `<KEY>:open`, `<KEY>:deposit`. A key is
1-128 letters, digits and `: _ . -`. A key seen before answers with its first result, so a
new ask always gets a new time, even an ask just like an earlier one. On an error, a
timeout or an unclear answer, **do not re-run** the command: a second open is a second
trade. Check it with `acp site status <KEY>:<STEP>`. A card that ended `unknown` is never
filed again until step 1 shows what happened.

## Failure handling

A `not_done` names what went wrong; `references/failures.md` says what to do for each of
Variational's own (a turned-down order, a wrong access code, a raised fee, a closed market,
a size). `SITE_*` codes come with their own message: relay it.

## Limits

- **Markets:** any Variational lists. Swaps and stock perps keep market hours.
- **Sizes:** Variational's own minimum (the read's `Market` line, per market and side, not
  Hyperliquid's $15) and maximum order size.
- **Leverage:** 1 to 5. **Margin:** cross unless they ask for isolated, and a position keeps
  its mode.
- **Money moves:** a deposit is USDC from Butler's wallet on Arbitrum. A withdrawal goes
  back to Butler's wallet only.

## Say to the owner

- Size in dollars and leverage together: "a $150 ETH long with $50 of your money, at 3x".
- Approval cards: what it does, then "approve it in the app".
- A sign-up card: "it asks for your Variational access code: type it on the card, not here".
  No code: someone who uses Variational, or https://discord.gg/variational (`references/setup.md`).
- After a close or withdrawal: what the outcome says, and nothing else.
- After a deposit or withdrawal, a gap the size of the card's fee is Variational's fee:
  say so, never call it missing money.
- A read in plain words, from this turn's read: "$12.40 on Variational, no open positions".
- A liquidation as the read says it: "about $69,476, 13% below the price now".
- Never the browser, the server, an idempotency key or an approval id.
