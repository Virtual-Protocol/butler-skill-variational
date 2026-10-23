# Changelog

## 1.0.0

- First version. The playbook that was six rows in the built-in `acp` skill
  (virtuals-agent `skill-md.ts`), as a hub skill: read the account and resting orders with
  `bevo-read get /variational/*`, then file each act with `acp variational …` and follow
  its card with `bevo-read request <key> --route variational`.
- Every command in `references/commands.md`, from bevo-server `docs/variational.md`.
- The first card on a new account comes back for the access code; the owner enters it on
  the card (`references/setup.md`).
