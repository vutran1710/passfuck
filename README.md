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
| Browser | Browserpass toolbar icon → type the alias → fill |
| Any Mac app | Raycast → "Find in Pass" → alias → copy / paste |
| iPhone | Keyboard AutoFill → Passwords → Pass → search alias; or open the app, tap to copy |
| Terminal | `pass -c gh`, `pass edit bank-main`, `pass generate -c work/mail 24` |

Entries are named by **aliases you choose** (`gh`, `bank-main`, `work/mail`), not by site.
The real site, username and password are encrypted inside each entry (`url:`, `login:` lines),
so the public vault never lists which sites you use. Aliases are permanent in git history.

## iPhone

Uses the existing free app [Pass - Password Store](https://apps.apple.com/app/pass-password-store/id1205820573)
(open source, [mssun/passforios](https://github.com/mssun/passforios)); nothing to build.
Run `pf phone`, AirDrop the folder, follow its `README.txt`, then enable Pass in
Settings → General → AutoFill & Passwords. Verified: the app's crypto library (GopenPGP
`v2.10.0-passforios`) imports the phrase-derived key and decrypts/encrypts entries compatibly.

Every vault change is committed and pushed automatically; a background job pulls every 10 min.

## Other commands

```sh
pf import ~/Downloads/passwords.csv   # Chrome/Brave/Safari/Apple/Firefox/1Password export; asks an alias per entry
pf phone                              # bundle key + deploy key for AirDrop to the iPhone
pf init                               # change the master phrase (re-encrypts everything)
```

`docs/demo/stupid-app.html` ([live](https://vutran1710.github.io/passfuck/demo/stupid-app.html)) is a login page with no autofill hints, for testing.

## Security model

- Vault contents (sites, passwords, usernames, notes) are encrypted; **aliases, entry count and history are public.**
- Anyone can download the vault and guess phrases offline at ~10 s and 1 GB of RAM per guess.
  The phrase must be long and invented — never a quote, lyric or personal fact.
- On each device the key is stored protected by the same phrase (GnuPG S2K, weaker than Argon2id),
  so a stolen laptop/phone key file is crackable faster than the vault. Use disk encryption.
- Lose the phrase → lose everything. Leak the phrase → leak everything.
