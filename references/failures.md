# Variational's answers, and what to do

A card or read that ends `not_done` says why. Find the line below; nothing moved unless it
says so. `SITE_*` codes come with their own message: relay it.

| Answer | What to do |
| --- | --- |
| `not_done`: no Variational account yet | Step 4: the `signup` card, then the original ask with a new key. |
| `not_done`: "That access code didn't work" | Nothing was sent. Tell your owner, and file a new `signup` card only when they ask to try another code; never on your own. With no other code, say where one comes from (`references/setup.md`, No code yet). |
| `not_done`: "Couldn't set up Variational right now" | The code wasn't used. Try again later. |
| `not_done`: less USDC on Arbitrum than the deposit | Nothing moved. Step 2's deposit check, then file it again with a new key. |
| `not_done`: "Variational's fee is now … more than the … on the card" | Nothing moved: Variational raised its fee. Read step 1 again, tell your owner the new fee, and file again with it and a new key if they still want it. |
| `not_done` naming a closed or close-only market | Swaps and stock perps keep market hours; nothing moved. Say so. Next time step 1's `--market` says it before any card. |
| `not_done` naming a size | Below Variational's minimum, or more than the position; nothing moved. Quote the minimum it names. |
| `not_done`: "Variational took the order, then failed …" or "refused the order: …" | Nothing moved: Variational turned the order down, usually because the account cannot back it at that leverage. Say so with its reason, read step 1 again with the same `--side` and size, and offer what its `Estimate` line says fits. |
| `not_done`: "could not reach Variational from the browser" | Nothing was signed or sent. Try once more in a minute; if it fails again, say Variational can't be reached right now. |
| `not_done`: "Variational now estimates liquidation at … much closer than … on the card" | Nothing was sent: the position would sit much nearer liquidation than the card showed. Tell your owner both figures, read step 1 again with the open's side and size, and file a new card only if they still want it. |
| `SITE_BAD_ARGS` | Fix the command from its message (`references/commands.md`); nothing was filed. |
