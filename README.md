# Blue Public

A Minecraft Java Edition 1.8.9-compatible client written from scratch in Rust.

> **Status:** closed beta. Access is gated on a Discord role and a hardware-bound
> licence token issued by our authority service. Source for the client is not
> public.

## Goals

- Faithful 1.8.9 movement, physics, and packet behaviour for serious competitive
  PvP. The client is designed to behave like a vanilla 1.8.9 client; nothing
  about it is intended to give the user an in-game advantage.
- Modern rendering pipeline (`wgpu`) with the same camera matrices, projection,
  and field-of-view as vanilla.
- Strict authentication: no cracked-account path, no offline-name multiplayer.
  Multiplayer refuses to connect without a Microsoft access token.

This binary does **not** redistribute any Mojang code, textures, sounds, or
models. Those assets are loaded from the user's own legally obtained Minecraft
installation when the client runs.

## What you need

- Windows 10/11 x86_64, or Linux x86_64. macOS support is best-effort.
- A legitimately purchased Microsoft Minecraft Java Edition account.
- Membership in the project's Discord server with the **Beta Tester** role.

## Getting started

1. Download the latest release for your platform from the
   [Releases](https://github.com/lilify-jp/BluePublic/releases) page.
2. Unzip if applicable.
3. Run the executable.
4. The client will:
   - Prompt you to accept the [EULA](EULA.md) and [Privacy Policy](PRIVACY.md).
   - Open your browser to the Discord OAuth flow.
   - On success, store a hardware-bound licence token under your OS config
     directory.
   - Boot to the main menu.
5. Sign in with your Microsoft account from the in-game menu.
6. Connect to a server.

If sign-in fails, the on-screen message tells you why. The most common cases
are "Microsoft account sign-in required" and "access denied: user lacks
required beta role".

## Files installed

| Path                                       | Purpose                                          |
|--------------------------------------------|--------------------------------------------------|
| `<config-dir>/bluepublic/token.json`       | Cached licence token (HWID-bound, restricted permissions). |
| `<config-dir>/bluepublic/state.json`       | Recorded EULA acceptance.                        |
| `<config-dir>/bluepublic/logs/*.log`       | Daily-rotated diagnostic logs (14-day retention).|

`<config-dir>` resolves to `%APPDATA%` on Windows, `$XDG_CONFIG_HOME` on Linux,
and `~/Library/Application Support` on macOS.

## Reporting bugs

1. Reproduce the bug with default settings if you can.
2. Attach the most recent two log files from `<config-dir>/bluepublic/logs/`.
   They are scrubbed of credentials before being written.
3. Open a new issue: https://github.com/lilify-jp/BluePublic/issues/new/choose

## Compliance & legal

- Blue Public is **not** affiliated with, endorsed by, or sponsored by Mojang
  Studios or Microsoft. *Minecraft* is a trademark of Mojang Synergies AB.
- Mojang's textures, sounds, models, and game code are loaded from the user's
  own legally obtained Minecraft installation; nothing of Mojang's is
  redistributed in this binary.
- Use of this client is governed by [EULA.md](EULA.md) and [PRIVACY.md](PRIVACY.md).
- Source code for the client is **not** distributed and is **not** licensed
  under MIT, Apache, GPL, or any other open-source licence. See
  [LICENSE](LICENSE).

## Security disclosure

Please **do not** open a public GitHub issue for security vulnerabilities.
Contact us privately via Discord direct message or the email listed on the
project's Discord server. We aim to acknowledge security reports within
72 hours.

## Acknowledgements

The Rust ecosystem maintainers — particularly `wgpu`, `tokio`, `winit`, and
`bevy_ecs` — without which this would not be feasible.
