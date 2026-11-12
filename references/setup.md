# Setting up Variational: the `signup` card

Butler's wallet needs a Variational account before it can trade. Variational needs no
access code any more (it opened to everyone), and if it ever asks for one the server fills
it in: your owner needs no code, and neither do you.

1. A deposit or an open on a wallet with no account ends `not_done`, saying Butler's
   wallet has no Variational account yet. Nothing was sent.
2. File the sign-up, with its own key:

   ```sh
   acp site variational signup --idempotency-key <KEY>:signup
   ```

3. Your owner holds to approve its card. Approving accepts Variational's Terms of Use (the
   card's `Terms` row); nothing is deposited or traded.
4. Once `acp site status <KEY>:signup` says `done`, file the original ask again, with a new
   errand key (a new time in it).

What you say: "First time on Variational. Approve the sign-up card in the app; it accepts
Variational's Terms." Never ask for an access code. If your owner offers one, say none is
needed, and don't repeat it or put it in a command.

How it can end:

- `done`: "Variational is set up for Butler's wallet."
- `not_done`: "Couldn't set up Variational right now." Nothing was set up. Tell your owner
  to try again later; file `signup` again, with a new errand key, only when they ask. Never
  file it on your own, and never more than once per ask.

A close, withdrawal, margin, TP/SL or cancel on a wallet with no account is not set up: it
ends `not_done`, since there is nothing to act on.
