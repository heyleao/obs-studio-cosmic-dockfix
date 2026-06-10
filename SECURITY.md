# Security Policy

## Supported scope

This repository packages OBS Studio with local patches for COSMIC/XWayland dock
behavior. It does not replace upstream OBS Studio security handling.

Report security issues in OBS Studio, CEF, Chromium, Qt, or system libraries to
their upstream projects first.

## Reporting package issues

For issues specific to this package, open a GitHub issue without including
private credentials, stream keys, OAuth tokens, logs containing secrets, or
personal account data.

Do not paste Twitch stream keys, cookies, browser profile data, or OAuth
credentials into public issues.

## OAuth credentials

The public package does not ship private OBS OAuth credentials. If you build
with `TWITCH_CLIENTID`, `TWITCH_HASH`, or other OAuth-related variables, treat
those values as secrets unless you intentionally created them for public use.
