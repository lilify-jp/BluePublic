# Blue Public — Privacy Policy

**Version:** 2026-05-10
**Effective date:** 2026-05-10

This document tells you, plainly, what data the Blue Public client and its
associated services collect, why we collect it, where it goes, and how to
ask us to delete it.

This policy is not a substitute for legal advice. If you have specific
compliance obligations (GDPR, CCPA, APPI, etc.) you are responsible for
reviewing this document against them before deciding whether to use the
Software or purchase any tier or cosmetic.

---

## 1. Who we are

The Blue Public project ("we"). Contact: the project's Discord server, or via
GitHub Issues at https://github.com/lilify-jp/BluePublic.

We are not a registered legal entity. There is no commercial product behind
the Software in the corporate-veil sense, but we do accept payment for tiers
and cosmetics; payment processing is handled entirely by the third-party
storefronts listed in section 5.

## 2. What data the client collects

When you run the Blue Public client, it processes the following data
**locally on your machine**:

| Item                      | Source                                                | Purpose                                |
|---------------------------|-------------------------------------------------------|----------------------------------------|
| Minecraft username        | Microsoft account auth                                | Display name, multiplayer connect      |
| Microsoft account UUID    | Microsoft account auth                                | Multiplayer auth                       |
| Microsoft access token    | Microsoft account auth                                | Multiplayer auth (held in memory)      |
| Hardware fingerprint hash | Computed from machine-id + MAC + home-directory path  | Bind licence token to this machine     |
| Settings file             | Your edits                                            | Persist game preferences               |
| Local game logs           | The client                                            | Diagnostic logs for bug reports        |
| Crash dumps (optional)    | Sentry SDK, if `SENTRY_DSN` is configured             | Crash diagnostics                      |

The Microsoft access token never leaves your machine except to talk to
Microsoft and to the Minecraft server you join. The hardware fingerprint
hash is sent only to our licence-authority service, only as a SHA-256 hash,
and only during licence checks.

## 3. What our licence-authority service collects

When you authenticate against the licence service, we record:

- The hash of your hardware fingerprint (never the raw value).
- The user identifier returned by the upstream auth provider (Discord ID, or
  a synthetic dummy identifier in development).
- The IP address you connect from, for the duration of session validation,
  plus a trailing five-minute window for rate-limit accounting.
- A randomly generated session identifier and (after success) a JWT identifier.
- The issuance and expiry timestamps of the JWT.

We do **not** store:

- Your Microsoft account.
- Your Minecraft username.
- The contents of any chat, in-game action, or world data.
- The raw hardware fingerprint string.

## 4. What our entitlement / billing data looks like

When you obtain Lifetime Beta Access or a cosmetic, we record:

| Item                       | Source                          | Purpose                                  |
|----------------------------|---------------------------------|------------------------------------------|
| Discord user ID            | Discord OAuth                   | Match the entitlement to your account    |
| Storefront order reference | BOOTH / Gumroad / future        | Audit trail, refund verification         |
| SKU                        | Our catalogue                   | Identify what you bought                 |
| Granted-at timestamp       | Server clock                    | Audit trail                              |
| Optional `expires_at`      | Subscription endpoint           | Subscription cycle (if applicable)       |

We do **not** record, and never see:

- Your full name (unless you happen to give it to us in a support ticket).
- Your email address (unless you contact us with it).
- Your billing address.
- Your credit card number, bank account, or any other payment instrument
  detail. The storefront you purchased from holds those; for them, see
  their own privacy policy.

## 5. Storefronts and other third parties we route data through

| Service                  | Receives                                       | Lives at                          |
|--------------------------|------------------------------------------------|-----------------------------------|
| Discord                  | OAuth identification + role lookups            | https://discord.com/privacy       |
| BOOTH (PIXIV Inc.)       | Payment for Lifetime Beta Access / cosmetics   | https://booth.pm/privacy          |
| Gumroad                  | Payment for Lifetime Beta Access / cosmetics   | https://gumroad.com/privacy       |
| Sentry                   | Crash events (only if `SENTRY_DSN` is set)     | https://sentry.io/privacy         |
| Microsoft                | Game account authentication                    | https://privacy.microsoft.com/    |

## 6. What our crash reporter collects

If `SENTRY_DSN` is configured at the time the client is built or run, the
Sentry SDK sends crash events to a Sentry instance under our control. Each
event contains:

- A stack trace.
- The Rust panic message, with credential-shaped substrings (`token`,
  `hwid`, `Authorization`, `Bearer …`, `password`, `secret`, and any path
  containing `/users/` or `\\users\\`) replaced with `[redacted]` before
  transmission.
- Operating system, Rust version, and Blue Public release version.

Sentry's "default PII" feature is **disabled**. Username, server name, and
IP address are stripped from each event before sending.

If you do not want crash data sent, do not run a build configured with a
`SENTRY_DSN`. The client functions normally without it.

## 7. What our auto-updater collects

The auto-updater issues an HTTPS request to our licence-authority service to
fetch the release manifest. The request itself is logged at the licence
service in the same access-log line as a licence call (IP address,
timestamp, user-agent). No additional information is collected by the
updater.

## 8. Data retention

| Item                                | Retention                                              |
|-------------------------------------|--------------------------------------------------------|
| Hardware fingerprint hash + tokens  | Until your token expires + 24 hours (audit), then deleted |
| Rate-limit / IP attempt counters    | 5 minutes (in-memory, lost on restart)                 |
| Crash events (Sentry)               | 90 days, default Sentry retention                      |
| Local game logs (your machine)      | 14 days, rotated automatically                         |
| Local settings + acceptance record  | Until you uninstall                                    |
| Entitlement records (paid tiers, cosmetics) | While the entitlement is in force, plus seven (7) years after the last related transaction — required by the Japanese Companies Act and Income Tax Act for tax-record retention |
| Purchase audit references           | Same seven-year retention as above                     |

## 9. Where the data goes

- The licence service runs on a single VPS we operate. Data does not leave
  that VPS except in encrypted backups under our control.
- Sentry crash events go to the Sentry instance configured at build time.
- Storefront purchase data sits at the storefront (BOOTH or Gumroad). We
  receive a webhook with the order reference and the buyer's Discord
  identifier; we do not pull additional fields from the storefront unless
  required to verify a refund.
- We do **not** sell or share your data with advertisers, data brokers, or
  any third party for marketing purposes.

## 10. Your rights

Wherever applicable law gives you the right to:

- Ask what data we hold about you.
- Ask us to correct it.
- Ask us to delete it.
- Withdraw consent for any processing that requires consent.
- Receive a portable export.

…you can exercise those rights by contacting us through the channels in
section 1. For deletion requests we need the user identifier the licence
service issued to you (visible in the client during authentication).

**Tax-record exception.** Where you have purchased a paid tier or cosmetic,
we are required by Japanese law (Companies Act art. 432; Income Tax Act
art. 232) to retain the transaction record for **seven years** after the
end of the fiscal year in which the transaction occurred. During that
period we cannot delete the entitlement and order-reference rows even on
request, but we will redact identifying fields (Discord user ID, IP) where
the law allows.

For deletion of all other data we will act within 30 days.

## 11. Security

We use TLS for all data in transit. The licence service stores data in a
SQLite database on disk; the JWT signing secret is held in an environment
file with restricted file-system permissions. Our threat model assumes
that anyone with shell access to the VPS can read everything, so we do
not store anything we would not want a system administrator to see.

## 12. Children

The Software is not directed at children under 13. We do not knowingly
collect data from anyone under 13. If you believe we have, contact us —
we will delete it (subject to the tax-record exception in section 10 if
the child made a purchase, in which case we will refund the purchase).

## 13. Changes to this policy

Material changes are signalled by bumping the version stamp at the top of
this document. The client will re-prompt you to accept the new version
before continuing.

## 14. Complaints

If you believe we have mishandled your data, please contact us first so we
can fix it. You retain the right to lodge a complaint with the
data-protection authority in your jurisdiction (in Japan, this is the
Personal Information Protection Commission, https://www.ppc.go.jp/en/ ).
