# passfuck

Tooling for my password vault. The vault itself is a separate public repo that
contains nothing but encrypted entries:
[vutran1710/passfuck-vault](https://github.com/vutran1710/passfuck-vault).

**Only one secret exists: a master phrase in my head.** It deterministically
regenerates the GPG key ([passphrase2pgp](https://github.com/skeeto/passphrase2pgp):
Argon2id, 1 GB, salted with the user ID in `pf.conf`). No key file to back up.

Built entirely from existing tools: [`pass`](https://www.passwordstore.org/) ·
[Browserpass](https://github.com/browserpass/browserpass-extension) ·
[Raycast Pass](https://www.raycast.com/) · [Pass for iOS](https://github.com/mssun/passforios).

## Setup (first time, or any new Mac)

```sh
curl -fsSL https://vutran1710.github.io/passfuck/install | bash
```

Website: <https://vutran1710.github.io/passfuck/>. After installing, `pf` is on your PATH.

`pf` with no arguments is a wizard: master phrase → this Mac → browser extension →
Raycast → import existing passwords → iPhone. Safe to re-run; done steps are skipped.

## Daily use

| Where | How |
|---|---|
| Browser | Browserpass toolbar icon (or its keyboard shortcut) → fill |
| Any Mac app | Raycast → "Find in Pass" → copy / paste |
| iPhone | Keyboard AutoFill → Passwords → Pass; or open the app, search, tap to copy |
| Terminal | `pass show github.com`, `pass -c github.com`, `pass generate -c newsite.com 24` |

Every vault change is committed and pushed automatically; a background job pulls every 10 min.

## Other commands

```sh
pf import ~/Downloads/passwords.csv   # Chrome/Brave/Safari/Apple/Firefox/1Password export
pf phone                              # bundle key + deploy key for AirDrop to the iPhone
pf init                               # change the master phrase (re-encrypts everything)
```

`demo/stupid-app.html` is a login page with no autofill hints, for testing.

## Security model

- Vault contents (passwords, usernames, notes) are encrypted; **filenames (site names) and history are public.**
- Anyone can download the vault and guess phrases offline at ~10 s and 1 GB of RAM per guess.
  The phrase must be long and invented — never a quote, lyric or personal fact.
- On each device the key is stored protected by the same phrase (GnuPG S2K, weaker than Argon2id),
  so a stolen laptop/phone key file is crackable faster than the vault. Use disk encryption.
- Lose the phrase → lose everything. Leak the phrase → leak everything.
