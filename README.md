# gst-wayland-display

The `gst-wayland-display` GStreamer plugin — the `waylanddisplaysrc` element that
acts as the Wayland parent for a nested compositor.

This candy builds and installs `libgstwaylanddisplaysrc.so`. The element embeds a
Smithay compositor, creates `zwp_linux_dmabuf_v1` on a real DRM render node, and
produces GStreamer buffers straight out of the compositor — which is what lets a
nested Hyprland run with no seat, no KMS device, and no DRM master.

It is built from the [opencharly fork](https://github.com/opencharly/gst-wayland-display)
at a pinned commit rather than from crates.io: the fork carries the resize
sentinel fix, the `xkb-layout`/`xkb-variant`/`xkb-options` element properties,
and smooth-scroll axis support. Upstream has none of these. The source is fetched
at an immutable commit tarball, built with cargo, and only the resulting `.so` is
installed — the build tree is removed in the same step.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `gst-wayland-display` |
| Distro | all (built from the pinned fork tarball; `base-devel` on arch/cachyos) |
| Artifact | `/usr/lib/gstreamer-1.0/libgstwaylanddisplaysrc.so` (`waylanddisplaysrc` element) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-streamed-desktop:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-gst-wayland-display:v2026.241.2202'
```

Then, inside the built image:

```bash
gst-inspect-1.0 waylanddisplaysrc   # registers the element
gst-inspect-1.0 waylanddisplaysrc | grep xkb-layout
```

## Layout

- `charly.yml` — the `gst-wayland-display:` candy entity: the pinned fork
  commit, the cargo build step, and the `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-distros:omarchy-cstream` — the streamed desktop whose
  transport spine this plugin is the Wayland parent for (this candy carries no
  `skill:` entity of its own)
- `/charly-selkies:selkies` — the other browser-desktop transport
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
