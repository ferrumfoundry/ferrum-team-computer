# Build Notes — Ferrum Team Computer package

Built locally only. Nothing was pushed, no GitHub repo was created, and
no credentials were used or needed.

## Design (as directed)

- **No passwords anywhere.** There is no repo secret, no password
  variable, and no step that sets a password — for Jakob or the bots.
- **Linux:** XFCE desktop + LightDM configured for auto-login as user
  `ferrum` (created by `useradd` with no password flag, so the account's
  password field stays locked). The live session runs on display :0 and
  is served by **x11vnc with `-nopw`** on port 5900, tunneled by bore.
  Connect with a free VNC app; no password prompt.
- **Windows:** local admin user `ferrum` created with `New-LocalUser
  -NoPassword`; `LimitBlankPasswordUse=0` set so a blank password is
  accepted over the network; Network Level Authentication turned off
  (it rejects blank passwords before sign-in); RDP enabled on 3389,
  tunneled by bore. Connect with Microsoft Remote Desktop, user
  `ferrum`, password left empty.
- **Tunnel:** bore (github.com/ekzhang/bore) instead of ngrok, because
  ngrok now requires a credit card for TCP tunnels. Bore needs no
  account and no card.
- Both workflows are `workflow_dispatch` only, can run at the same
  time, heartbeat every 30 minutes, and wind down at ~5h50m, under
  GitHub's 6-hour job cap (job timeout set to 355 minutes).

## Validated locally

- **YAML:** both workflow files parse clean with a real parser
  (PyYAML). Triggers confirmed as `workflow_dispatch` only; job
  timeouts and step lists confirmed. (PyYAML reads the bare key `on`
  as boolean `True` — a known YAML 1.1 quirk; GitHub's parser reads
  it as the trigger, which is the standard form for Actions files.)
- **Bash syntax:** all five `run:` blocks of the Linux workflow were
  extracted and pass `bash -n`.
- **No-password greps** over the whole package — all came back empty:
  `secrets.` references, `TEAM_PC_PASSWORD`, `chpasswd`,
  `SecureString` / `ConvertTo-SecureString`, and any password variable
  or assignment pattern (`$...password`, `PASSWORD=`, `password=`).
- **Full word sweep for "password":** every remaining hit is one of —
  (a) prose/comments stating there is no password, (b) the
  `-NoPassword` parameter of `New-LocalUser`, which is the switch that
  *creates* a passwordless account, or (c) `LimitBlankPasswordUse`,
  which is Windows' own registry value name and is required by the
  design. No password value exists anywhere in the package.
- **Bore release assets verified real** against the GitHub API
  (ekzhang/bore latest release, v0.6.0) and by downloading both
  archives and listing their contents:
  - Linux: `bore-v0.6.0-x86_64-unknown-linux-musl.tar.gz` — contains
    a single `bore` binary.
    https://github.com/ekzhang/bore/releases/download/v0.6.0/bore-v0.6.0-x86_64-unknown-linux-musl.tar.gz
  - Windows: `bore-v0.6.0-x86_64-pc-windows-msvc.zip` — contains a
    single `bore.exe`.
    https://github.com/ekzhang/bore/releases/download/v0.6.0/bore-v0.6.0-x86_64-pc-windows-msvc.zip
  The URLs in the workflows match these exactly.

## Not verified (needs a live run — stated plainly)

- **Neither workflow has ever run on GitHub.** Everything above is
  static validation only.
- **Bore tunnel end-to-end:** a test connection to bore.pub from this
  build machine timed out (this sandbox's network blocks bore.pub's
  control port, 7835), so the tunnel itself is unproven from here.
  GitHub runners are on a different network; the first live run is
  the real test. The workflows fail loudly and print bore's log if no
  address is published.
- **Linux desktop session:** LightDM auto-login is configured as
  designed, but GitHub's runners are headless (no physical display,
  no systemd), so the workflow brings the XFCE session up manually
  under Xvfb on :0 and serves that with x11vnc. Whether the full
  desktop + VNC behaves end-to-end (panel, file manager, session
  stability over hours) is unproven until a live run.
- **Blank-password RDP:** `LimitBlankPasswordUse=0` + NLA off is the
  documented recipe for RDP with an empty password, but whether the
  Microsoft Remote Desktop phone app connects smoothly with an empty
  password field is unproven until tried on a live machine.
- **PowerShell syntax:** no PowerShell exists on this build machine,
  so the Windows workflow's scripts were reviewed by eye only (the
  Linux side got a real `bash -n` check). First live run may surface
  a typo static review missed.
- **Windows Defender** on the runner could, in principle, flag the
  downloaded `bore.exe`; unknown until a live run.
