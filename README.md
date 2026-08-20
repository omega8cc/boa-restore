# boa-restore

Read-only access to your [BOA](https://github.com/omega8cc/boa) off-site
backups, from your own computer. It works like the server-side `mybackup`
tool, just locally: it never creates, prunes or modifies backups — it can
only look at them and restore from them.

Full walkthrough (who it's for, getting your passphrase, the fire drill):
**[Disaster-proof access to your off-site backups](https://docs.boa.io/using/backups/disaster-proof-restore)**
on the BOA docs site.

## What you need

- **Duplicity 2.0 or newer** (3.x recommended) — `brew install duplicity`
  on macOS, `sudo apt install duplicity` on current Debian/Ubuntu, or
  `pipx install duplicity` anywhere. On Windows, install WSL first
  (`wsl --install` in an Administrator PowerShell) and work inside it.
- Your **backup passphrase** — one support request to your host; the docs
  page above has the exact wording.
- Your **storage credentials** — the same values as in your server-side
  `~/static/control/remote_backups/credentials/<service>.txt` file.

## Install

```sh
curl -fsSL https://raw.githubusercontent.com/omega8cc/boa-restore/main/boa-restore -o ~/boa-restore
```

No further installation — run it with `bash ~/boa-restore ...`.

## Use

```sh
bash ~/boa-restore setup
```

creates `~/.boa-restore/config.txt` with instructions inside: fill in your
account, server and service, paste your provider credential lines, and
save your passphrase into `~/.boa-restore/.secret.txt`. Then:

```sh
bash ~/boa-restore check                # prove credentials, bucket and passphrase
bash ~/boa-restore list                 # list the files in the newest backup
bash ~/boa-restore restore              # restore everything
bash ~/boa-restore restore data/disk/o1/static/projects       # one folder
bash ~/boa-restore restore data/disk/o1/static/projects 7D    # as of a week ago
```

Restores always land in a fresh folder under `~/boa-restored/` — nothing
is ever overwritten. Restore paths are written the way the backup stores
them: relative to the server's filesystem root, with no leading slash.

Supported services: `aws`, `aws_one_zone`, `aws_standard_ia`, `azure`,
`b2`, `cloudflare`, `do_spaces`, `linode`, `wasabi`.

## Licence

GNU GPL version 2 or later. Copyright (C) 2009-2026
[Omega8.cc](https://omega8.cc).
