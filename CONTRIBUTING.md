# Contributing

## Goals

This package is a pragmatic workaround for OBS Studio dock issues on COSMIC
Wayland/XWayland, especially with NVIDIA and browser docks.

Keep changes focused on:

- OBS packaging for Arch/CachyOS-style systems.
- COSMIC/XWayland dock behavior.
- CEF/obs-browser launch behavior needed by this workaround.
- Clear documentation for users testing the package.

## Testing

Before proposing changes, run:

```sh
makepkg --verifysource
makepkg -C -o
makepkg -C -f
```

After installing the package, validate:

```sh
obs-studio-cosmic-dockfix
obs --version
```

On COSMIC/XWayland, `wmctrl -l -x -G` should show the main OBS window without
separate stuck top-level windows for native docks like Stats.

## OAuth and Twitch login

Do not submit private OAuth credentials. The package intentionally supports
optional build-time variables for people who have compatible credentials, but
the public repository must remain safe to fork and rebuild.
