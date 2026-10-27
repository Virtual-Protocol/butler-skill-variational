# Changelog

## 2.0.0

- On Bevo's site rail: every act is `acp site variational <action>`, the reads are
  `acp site variational account` and `orders`, and a card is followed with
  `acp site status <key>`. The rail's own output and codes say what to do; this skill adds
  only what is Variational's.
- Sign-up is its own card (`signup`); the owner types the access code on it.
- A deposit the wallet can't cover ends `not_done` after approval: read the wallet first.
- No pushed fill notes: `orders` lists resting orders and those that filled or were
  cancelled in a day.
- `references/flows.md`: errands that take more than one card.

## 1.0.1

- A deposit the wallet can't cover is refused before any card
  (`VARIATIONAL_INSUFFICIENT_USDC`): read every chain's USDC before offering a bridge, and
  never offer more than a chain holds.
- The account read is relayed in plain words, never field names: a wallet with no account
  answers `hasAccount: false` with a `hint` to say instead.

## 1.0.0

- First version. The playbook that was six rows in the built-in `acp` skill
  (virtuals-agent `skill-md.ts`), as a hub skill: read the account and resting orders with
  `bevo-read get /variational/*`, then file each act with `acp variational …` and follow
  its card with `bevo-read request <key> --route variational`.
- Every command in `references/commands.md`, from bevo-server `docs/variational.md`.
- The first card on a new account comes back for the access code; the owner enters it on
  the card (`references/setup.md`).
