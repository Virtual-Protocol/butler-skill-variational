# Setting up Variational: the `signup` card

Butler's wallet needs a Variational account before it can trade, and creating one takes an
**access code** from Variational. The owner has it; Butler never sees it.

1. A deposit or an open on a wallet with no account ends `not_done`, saying Butler's
   wallet has no Variational account yet. Nothing was sent.
2. File the sign-up, with its own key:

   ```sh
   acp site variational signup --idempotency-key <KEY>:signup
   ```

3. Its card asks your owner for the code: they type it on the card and hold to approve.
   Approving also accepts Variational's Terms of Use (the card's `Terms` row).
4. Once `acp site status <KEY>:signup` says `done`, file the original ask again, with a new
   errand key (a new time in it).

What you say: "First time on Variational. Approve the sign-up card and type your access
code on it." Never ask for the code in chat. If they paste one anyway, don't repeat it,
don't use it, and tell them it goes on the card.

How it can end:

- `done`: "Variational is set up for Butler's wallet."
- `not_done`: "That access code didn't work." Nothing was sent. Tell your owner, then wait:
  file `signup` again, with a new errand key, only when they ask to try another code. Never
  file it on your own, and never more than once per ask.
- `not_done`: "Couldn't set up Variational right now." The code wasn't used; try later.

A close, withdrawal, margin, TP/SL or cancel on a wallet with no account is not set up: it
ends `not_done`, since there is nothing to act on.
