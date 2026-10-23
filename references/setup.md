# The first card: setting up Variational

Butler's wallet needs a Variational account before it can trade, and creating one takes an
**access code** from Variational. The owner has it; Butler never sees it.

Nothing to file for it. Signing in and signing up are never commands: every card signs in
by itself, unseen. When the wallet has no account yet:

1. File the act your owner asked for, as usual (a deposit or an open).
2. When they approve it, the card finds no account, stops before anything is signed, and
   comes back to them as **"Set up Variational to deposit 2 USDC"** (or "... to open ...").
   The chat gets a line saying so.
3. The owner taps **Enter access code** on the card, types the code, and holds to approve.
   That hold also accepts Variational's Terms.
4. The card then sets up the account and runs the act.

What you say: "First time on Variational. Enter your access code on the card to continue."
Never ask for the code in chat. If they paste one anyway, don't repeat it, don't use it,
and tell them it goes on the card.

How it can end:

- **"Variational is set up. Deposited 2 USDC."** - done.
- **"That access code didn't work. Nothing was deposited."** - the code was refused. If
  they want to try again, file the act again with a new key; they enter a new code on it.
- **"Couldn't set up Variational right now. Your code wasn't used."** - try again later.

A set-up card waiting more than 30 minutes for its code expires when approved, so an old
open never runs at a price nobody saw: file it again.

A close or a withdrawal on a wallet with no account is not set up: it fails plainly, since
there is nothing to close or withdraw.
