---
name: variational
description: Trade perps on Variational Omni from Butler's wallet - deposit, open, close, reverse, margin, limit, TP/SL, cancel, withdraw - each act a card the owner approves.
version: 1.0.0
metadata: {"butler":{"moneyMoving":true,"keywords":["variational","variational omni","omni.variational.io","trade on variational","long on variational","short on variational","variational position","close my variational","variational balance","deposit to variational","withdraw from variational","variational limit order","variational take profit","variational stop loss","cancel variational order"],"requires":{"bins":["acp","bevo-read"]}}}
---

## When to use

Your owner names **Variational** (Omni, omni.variational.io) and wants to:

- **Look** - "my Variational balance", "my positions there", "my orders": step 1, then stop.
- **Fund** - "put 20 USDC into Variational": step 3 with `deposit`.
- **Open** - "long ETH on Variational with $50 at 3x": steps 1 to 4.
- **Close, reverse, re-margin** - "close half my SOL", "flip it", "set my margin to 30": steps 1, 3, 4.
- **Wait for a price** - "buy ETH at 2,000", "stop loss at 2,300": steps 1, 3, 4, and
  `references/resting-orders.md`.
- **Withdraw** - "take my money out of Variational": step 3 with `withdraw`.

Not this skill: Hyperliquid perps (`acp trade`), Papertrade, or anything your owner did
not say is on Variational. Every act here is a card the owner approves; nothing moves
without it. Variational is only on for some accounts: a refusal says so, and you relay it.

## Before you start

1. **Variational is its own venue.** Its balance and positions are only in this skill's
   reads; Butler's wallet balance and Hyperliquid positions are something else.
2. **Figures come only from this turn's reads and the card.** An unreadable account is not
   an empty one. Never work out a liquidation price, a fee or a PnL yourself.
3. **Leverage is 1 to 5, and 1 unless they name one.** An open sets the leverage for the
   whole market, so it re-levers a position already open there: say so.
4. **The access code is never in chat.** The first card on a new account comes back
   asking for it, and the owner types it on the card (`references/setup.md`). Never ask
   for it, never repeat one they paste, and never file a sign-in or sign-up: every card
   does both by itself.
5. **From chat only.** A duty cannot use Variational; the server refuses it.

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
   bevo-read get /variational/account
   ```

   Relay it as given (`references/account.md`). `hasAccount: false` means Butler's wallet
   has no Variational account yet: a deposit or an open sets it up (step 4's card asks for
   the code); a close or withdrawal has nothing to act on. Each read answers at most once a
   minute: when told to wait, wait, never loop. A failed read: say you could not check it.
   For resting orders, read `bevo-read get /variational/orders` instead.

2. [ADAPT] Match the ask to the position you just read:
   - **Open on a market they hold on the same side**: it adds to that position, and the
     leverage applies to all of it. Say so.
   - **Open on the other side of a position they hold**: refused with nothing sent. Offer
     `close` or `reverse` instead.
   - **Close, reverse, margin, TP/SL**: only on a market they hold. `margin` needs an
     isolated position.
   - **Withdraw**: at most `withdrawable`.

3. [FIXED] File the card, once, with one key for this errand
   (`references/commands.md` has every form):

   ```sh
   acp variational open --market <SYMBOL> --usd <USD> --leverage <N> --side <SIDE> --idempotency-key <KEY>
   acp variational close --market <SYMBOL> --idempotency-key <KEY>
   acp variational reverse --market <SYMBOL> --idempotency-key <KEY>
   acp variational margin --market <SYMBOL> --amount <USDC> --idempotency-key <KEY>
   acp variational limit --market <SYMBOL> --side <SIDE> --usd <USD> --price <PRICE> --leverage <N> --idempotency-key <KEY>
   acp variational tpsl --market <SYMBOL> --tp <PRICE> --sl <PRICE> --idempotency-key <KEY>
   acp variational cancel --market <SYMBOL> --idempotency-key <KEY>
   acp variational deposit --amount <USDC> --idempotency-key <KEY>
   acp variational withdraw --amount <USDC> --idempotency-key <KEY>
   ```

   `<SYMBOL>` is what your owner named (`ETH`, `SOL`, `XAUS`, `AAPL`); the card checks that
   Variational lists it. `<SIDE>` is `long` or `short`. An open or limit sized in coins
   ("long 2 ETH") takes `--size <COINS>` instead of `--usd`; a part close adds
   `--percent <1-100>` or `--size <COINS>`. A limit, TP/SL or cancel: read
   `references/resting-orders.md` first for its extra flags. The reply is an approval card in this chat: tell your owner what it
   does, and to approve it in the app.

4. [FIXED] Wait for the card's outcome; never file it again to find out:

   ```sh
   bevo-read request <KEY> --route variational
   ```

   - `pending` - still with the owner: say so and stop until they come back.
   - `signed` - approved and running (a withdrawal stays here until it lands, up to about
     10 minutes). The chat is told when it ends.
   - `confirmed` - done: relay `approvalOutcome.summary` and its `facts` as written.
   - `failed` - give `approvalFailureReason` as written. "Check Variational before asking
     again" means the result is unknown: read step 1 before filing anything.
   - `rejected` or `expired` - nothing ran; say so.

   The server also posts how a card ended in this chat; relay that, don't add to it.

## Idempotency and retries

One key per errand, such as `var:eth:long:1`. On an error, a timeout or an unclear answer,
**do not re-run** `acp variational`: a second open is a second trade. Look it up as in step
4. A card that failed as unknown is never filed again until step 1 shows what happened.

## Failure handling

| Answer | What to do |
| --- | --- |
| `VARIATIONAL_NOT_ENABLED` | Not available on this account yet. Say so; don't retry. |
| `VARIATIONAL_SIGNER_NOT_READY` | Ask your owner to enable Butler's signer in the app, then file it again. |
| `VARIATIONAL_CHAT_ONLY` | You are in a duty. Variational is chat only; tell the owner in chat. |
| `VARIATIONAL_BAD_COMMAND` | Fix the command from its hint (`references/commands.md`); nothing was filed. |
| `VARIATIONAL_READ_BUSY` | Checked under a minute ago. Wait a minute; it is not an empty account. |
| `VARIATIONAL_READ_FAILED` | Could not read Variational. Say you could not check it. |
| A card failed with "nothing was sent" | Give the reason as written; nothing moved. |
| A card failed with "check Variational before asking again" | Unknown result: step 1, then tell the owner what the read shows. |
| "That access code didn't work" | The owner asks you again and enters a new code on the new card. |

## Limits

- **Markets:** any Variational lists. Swaps and stock perps keep market hours; a closed
  market stops the card with nothing sent. A close-only market can be closed, not opened.
- **Sizes:** Variational's own minimum and maximum order size. A close smaller than its
  minimum, or larger than the position, stops with nothing sent.
- **Leverage:** 1 to 5. **Margin:** cross unless they ask for isolated, and a position
  keeps its mode.
- **Money moves:** a deposit is USDC from Butler's wallet on Arbitrum, and stops before
  signing when the wallet holds less. A withdrawal goes back to Butler's wallet only.
  While a withdrawal is still landing, the next card waits.

## Say to the owner

- Size in dollars and leverage together: "a $150 ETH long with $50 of your money, at 3x".
- Approval cards: what it does, then "approve it in the app".
- A limit or TP/SL card: "placed", never "filled". A fill is told in chat when it happens.
- After a close or withdrawal: what the card's outcome says, and nothing else.
