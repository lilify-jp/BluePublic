# Blue Public

A Minecraft Java Edition 1.8.9-compatible client written from scratch in Rust.

> **Status:** closed beta. Access is gated on a Discord role and a hardware-bound
> licence token issued by our authority service. Source for the client is not
> currently public.

## Why?

Blue Public is the public release fork of an internal R&D client called *Blue*.
It exists to:

- Translate the 1.8.9 vanilla client to memory-safe Rust, line for line, so the
  movement, packet, and physics behaviour match the reference implementation.
- Replace the OpenGL pipeline with `wgpu` while keeping vanilla camera matrices,
  projection, and FOV identical.
- Serve as a Mojang-anti-cheat-friendly client for serious competitive PvP — no
  cracked-account path, no built-in cheats, no offline-name multiplayer.

The repository **does not contain** any decompiled Mojang source; the public binary
ships without it.

## What you need

- Windows 10/11 x86_64, or Linux x86_64. macOS support is best-effort.
- A legitimately purchased Microsoft Minecraft Java Edition account. Multiplayer
  refuses to connect without a Microsoft access token.
- Membership in the project's Discord server with the **Beta Tester** role granted.

## Getting started

1. Download the latest release for your platform from the
   [Releases](https://github.com/lilify-jp/BluePublic/releases) page.
2. Unzip if applicable.
3. Run the executable.
4. The client will:
   - Prompt you to accept the [EULA](EULA.md) and [Privacy Policy](PRIVACY.md).
   - Open your browser to the Discord OAuth flow.
   - On success, store a hardware-bound licence token at
     `<config-dir>/bluepublic/token.json` (mode `0600` on Unix).
   - Boot to the main menu.
5. Sign in with your Microsoft account from the in-game menu.
6. Connect to a server.

If sign-in fails, the error message tells you why — for example:
`Microsoft account sign-in required. Open the main menu and sign in before
joining a server.` or `access denied: user lacks required beta role`.

## Configuration

The client reads a small set of environment variables. None of them are required;
sensible defaults apply.

| Variable                       | Effect                                                    |
|--------------------------------|-----------------------------------------------------------|
| `BLUEPUBLIC_LICENSE_ORIGIN`    | Override the licence-server origin (testing only).        |
| `BLUEPUBLIC_DISABLE_UPDATER`   | Skip the auto-updater check at startup.                   |
| `BLUEPUBLIC_SKIP_TERMS`        | Skip the EULA prompt. **Do not set in production builds.**|
| `SENTRY_DSN`                   | If set, opt into crash reporting via Sentry.              |
| `BLUEPUBLIC_RELEASE`           | Override the release tag reported to crash reports.       |

## Files installed

| Path                                       | Purpose                                          |
|--------------------------------------------|--------------------------------------------------|
| `<config-dir>/bluepublic/token.json`       | Cached licence token (HWID-bound, `0600`).       |
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

- This client is not affiliated with, endorsed by, or sponsored by Mojang Studios
  or Microsoft. *Minecraft* is a trademark of Mojang Synergies AB.
- Mojang's textures, sounds, and code are loaded from your own legally obtained
  Minecraft installation; nothing of Mojang's is redistributed in this binary.
- Use of this client is governed by [EULA.md](EULA.md) and [PRIVACY.md](PRIVACY.md).

## Security disclosure

Please **do not** open a public GitHub issue for security vulnerabilities. Contact
us privately via Discord direct message or the email listed on the project's
Discord server. We aim to acknowledge security reports within 72 hours.

## Acknowledgements

- The Minecraft community for two decades of vanilla-physics archaeology.
- The Rust ecosystem maintainers — particularly `wgpu`, `tokio`, `winit`, and
  `bevy_ecs` — without which this would not be feasible.
