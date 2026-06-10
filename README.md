# OBS Studio COSMIC Dock Fix

OBS Studio package variant for COSMIC on Wayland/XWayland where Qt dock widgets
can become separate stuck top-level windows, especially with Twitch/browser
docks on NVIDIA.

This is a workaround package for Arch/CachyOS-style systems. It is not an
official OBS Studio build.

## What this package changes

- Launches OBS through Qt XCB/XWayland.
- Forces obs-browser/CEF to use X11 instead of mixing Wayland CEF with X11 Qt.
- Keeps browser hardware acceleration enabled on NVIDIA.
- Disables floating OBS docks so they stay embedded in the main window.

The tradeoff is intentional: dock pop-out/floating is disabled. Docks can still
be shown, hidden, and rearranged while embedded.

## Install

```sh
git clone https://github.com/heyleao/obs-studio-cosmic-dockfix.git
cd obs-studio-cosmic-dockfix
makepkg -si
```

Start the app with `OBS Studio COSMIC Dock Fix` from the launcher, or:

```sh
obs-studio-cosmic-dockfix
```

## Twitch login

Public rebuilds cannot ship OBS's private OAuth credentials. The default build
has browser panels but no native Twitch API login unless compatible credentials
are supplied at build time:

```sh
TWITCH_CLIENTID=... TWITCH_HASH=... makepkg -si
```

Without those values, use a stream key or external browser docks.

## Validate

```sh
obs --version
obs-studio-cosmic-dockfix
```

Expected version format:

```sh
OBS Studio - 32.1.2-cosmic-dockfix-1
```

On COSMIC/XWayland, `wmctrl -l -x -G` should show the main OBS window without
separate stuck top-level dock windows such as `Stats`/`Estatísticas`.

## Upstream context

The long-term fix likely belongs upstream in CEF/Chromium, Qt, OBS, or COSMIC.
Relevant CEF issue:

https://github.com/chromiumembedded/cef/issues/2804

## Security

Do not publish stream keys, cookies, OAuth tokens, or Twitch credentials in
issues or logs. See `SECURITY.md`.
