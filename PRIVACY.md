# Blue Public — Privacy Policy

**Version:** 2026-05-09
**Effective date:** 2026-05-09

This document tells you, plainly, what data the Blue Public client and its associated
services collect, why we collect it, where it goes, and how to ask us to delete it.

This policy is not a substitute for legal advice. If you have specific compliance
obligations (GDPR, CCPA, APPI, …) you are responsible for reviewing this document
against them before deciding whether to use the Software.

---

## 1. Who we are

The Blue Public project ("we"). Contact: the project's Discord server, or via GitHub
Issues at https://github.com/lilify-jp/BluePublic.

We are not a registered legal entity. There is no commercial product behind the Software.

## 2. What data the client collects

When you run the Blue Public client, it processes the following data **locally on your
machine**:

| Item                      | Source                                                | Purpose                                |
|---------------------------|-------------------------------------------------------|----------------------------------------|
| Minecraft username        | Microsoft account auth                                | Display name, multiplayer connect      |
| Microsoft account UUID    | Microsoft account auth                                | Multiplayer auth                       |
| Microsoft access token    | Microsoft account auth                                | Multiplayer auth (held in memory)      |
| Hardware fingerprint hash | Computed from machine-id + MAC + home-directory path  | Bind licence token to this machine     |
| Settings file             | Your edits                                            | Persist game preferences               |
| Local game logs           | The client                                            | Diagnostic logs for bug reports        |
| Crash dumps (optional)    | Sentry SDK, if `SENTRY_DSN` is configured             | Crash diagnostics                      |

The Microsoft access token never leaves your machine except to talk to Microsoft and
to the Minecraft server you join. The hardware fingerprint hash is sent only to our
licence-authority service, only as a SHA-256 hash, and only during licence checks.

## 3. What our licence-authority service collects

When you authenticate against the licence service, we record:

- The hash of your hardware fingerprint (never the raw value).
- The user identifier returned by the upstream auth provider (Discord ID once Discord
  auth is enabled, or a synthetic dummy identifier in development).
- The IP address you connect from, for the duration of session validation, plus a
  trailing five-minute window for rate-limit accounting.
- A randomly generated session identifier and (after success) a JWT identifier.
- The issuance and expiry timestamps of the JWT.

We do **not** store:

- Your Microsoft account.
- Your Minecraft username.
- The contents of any chat, in-game action, or world data.
- The raw hardware fingerprint string.

## 4. What our crash reporter collects

If `SENTRY_DSN` is configured at the time the client is built or run, the Sentry SDK
sends crash events to a Sentry instance under our control. Each event contains:

- A stack trace.
- The Rust panic message, with credential-shaped substrings (`token`, `hwid`,
  `Authorization`, `Bearer …`, `password`, `secret`, and any path containing `/users/`
  or `\\users\\`) replaced with `[redacted]` before transmission.
- Operating system, Rust version, and Blue Public release version.

Sentry's "default PII" feature is **disabled**. Username, server name, and IP address
are stripped from each event before sending.

If you do not want crash data sent, do not set `SENTRY_DSN` and do not run a build
configured with one. The client functions normally without it.

## 5. What our auto-updater collects

The auto-updater issues an HTTPS request to our licence-authority service to fetch the
release manifest. The request itself is logged at the licence service the same way
licence-validation requests are (IP address, timestamp, user-agent). No additional
information is collected by the updater.

## 6. Data retention

| Item                                | Retention                                                  |
|-------------------------------------|------------------------------------------------------------|
| Hardware fingerprint hash + tokens  | Until your token expires + 24 hours (audit trail), then deleted |
| Rate-limit / IP attempt counters    | 5 minutes                                                  |
| Crash events (Sentry)               | 90 days, default Sentry retention                          |
| Local game logs (your machine)      | 14 days, rotated automatically                             |
| Local settings + acceptance record  | Until you uninstall                                        |

## 7. Where the data goes

- The licence service runs on a single VPS we operate. Data does not leave that VPS
  except in encrypted backups under our control.
- Sentry crash events go to the Sentry instance configured at build time. If we use a
  hosted Sentry, the operator is functionally.sentry.io / sentry.io as listed at
  https://sentry.io/legal/dpa/.
- We do **not** sell or share your data with advertisers, data brokers, or any third
  party for marketing purposes.

## 8. Your rights

Wherever applicable law gives you the right to:

- Ask what data we hold about you.
- Ask us to correct it.
- Ask us to delete it.
- Withdraw consent for any processing that requires consent.

…you can exercise those rights by contacting us through the channels in section 1.
For deletion requests we need the user identifier the licence service issued to you
(visible in the client during authentication). On receipt we will delete the
corresponding rows from our database within 30 days, except where a longer retention
is required by law.

## 9. Security

We use TLS for all data in transit. The licence service stores data in a SQLite
database on disk; the JWT signing secret is held in an environment file with
restricted file-system permissions. Our threat model assumes that anyone with shell
access to the VPS can read everything; we therefore do not store anything we would not
want a system administrator to see.

## 10. Children

The Software is not directed at children under 13. We do not knowingly collect data
from anyone under 13. If you believe we have, contact us — we will delete it.

## 11. Changes to this policy

Material changes are signalled by bumping the version stamp at the top of this
document. The client will re-prompt you to accept the new version before continuing.

## 12. Complaints

If you believe we have mishandled your data, please contact us first so we can fix it.
You retain the right to lodge a complaint with the data-protection authority in your
jurisdiction.
