# AGENTS.md — layer-gst-wayland-display

Standalone candy repo for the `gst-wayland-display` layer — the
`libgstwaylanddisplaysrc.so` GStreamer plugin whose `waylanddisplaysrc` element
is the Wayland parent for a nested compositor, built from the opencharly fork at
a pinned commit. The candy lives in `charly.yml` at the repo root. It carries
**no `skill:` entity**, so no owning `/charly-<family>:<name>` skill is projected
into the marketplace corpus.

Canonical files:

- `charly.yml` — the `gst-wayland-display:` candy entity (no `skill:` entity
  present).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:omarchy-cstream` — the closest owning skill: the streamed
  Omarchy desktop whose transport spine this plugin is the Wayland parent for,
  and the `wl_compositor` v6 / `zwp_linux_dmabuf_v1` constraint it clears. Load
  before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections). Load before editing any entity field or plan step.

There is no dedicated `/charly-*:gst-wayland-display` owning skill — this repo's
candy carries no `skill:` entity. The gap is recorded against
`opencharly/opencharly#291`; when one is authored, add it here.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the `.so` file
  check, the `gst-inspect-1.0 waylanddisplaysrc` registration check, the three
  keymap-property checks, the DMABuf-negotiation check, and the render-node
  property check. The `check-gwd-layer` bed is the disposable build-scope run.
- The pinned fork commit appears **twice** — in the `download:` URL and in the
  extracted directory name — so a commit bump is a two-line change.

## Modify this repo

- Edit the `gst-wayland-display:` candy entity in `charly.yml`.
- Keep `set -e` WITHOUT `-u` in the build/gate commands: the env vars are
  legitimately unset in a build container, and charly reserves the brace-default
  form for its own runtime variables (a dollar-brace in a command is parsed as a
  charly runtime variable; unbound, the check SKIPS rather than fails).
- A fork-commit bump moves the `download:` URL and the extracted directory name
  together, plus the candy version.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
