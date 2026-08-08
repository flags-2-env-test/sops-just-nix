# sops-just-nix

Just+Nix port of the ORESoftware SOPS environment contract. The security behavior is intentionally inherited from the clean references:

- `flags-2-env-test/sops-just@933a239388449901bf8cccfd3db5c4d79fdec039`
- `flags-2-env-test/sops-nix@80ec3a48676ec366f0320b8b953008e57a9ef0bc`

This fixture has **no committed private age identity and no committed ciphertext**. Every run generates a fresh identity, exact dev/prod SOPS rules and synthetic ciphertext at runtime, validates the ignored decrypted state and managed root `.env` symlink, then removes all runtime state.

## What this variant adds

Just is deliberately only an invocation layer. `nix develop` supplies `just`, SOPS, age, Git, Python and shell/core utilities from the exact pinned nixpkgs revision, then `just verify` runs the same shared `scripts/assert.sh` contract. The direct `nix run .#verify` path is retained as a reference so wrapper drift is visible.

The same Just-inside-Nix path also runs inside the clean container boundary.

## Run

```sh
nix run .#verify
nix develop --command just verify
docker build -t sops-just-nix-runtime-fixture .
docker run --rm sops-just-nix-runtime-fixture
```

Tracking: DEN-2919 / DEN-2636.
