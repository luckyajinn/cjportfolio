# Handoff: portfolio redesign (as of 2026-11-04)

Notes for a new chat picking up this work on another device. Read this first, then `index.html` and `concepts/blueprint-b.html`.

## The site

- Live: https://luckyajinn.github.io/cjportfolio/ (GitHub Pages, deploys from `main`).
- `index.html` is the live page: one self-contained file, with all content in a `SITE` object in the script. Images and favicons are in `assets/`.
- The owner edits `index.html` directly on GitHub too, so run `git pull` before changing anything.

## What was done in the last session

1. Favicon links now point to `assets/favicon.ico`, `assets/favicon.svg` and `assets/apple-touch-icon.png`.
2. Added a faint CSS dot grid behind the hero (`.hero-grid`).
3. Added Schema.org Person JSON-LD to the `<head>` and a `<noscript>` summary after the hero.
4. Education: "Associate's, Computer Engineering", Santa Ana College, expected May 2028.
5. Built three creative redesign concepts in `concepts/`:
   - `terminal.html`: themed shell, command headings, optional command line, boot sequence. Dark only.
   - `blueprint.html`: network blueprint look with an invented hero diagram (superseded by `blueprint-b.html`).
   - `editor.html`: code editor workspace, file tabs, source/preview toggle, light and dark theme.
6. The owner picked the blueprint direction. Two real-homelab diagram variants were made; **variant B (`concepts/blueprint-b.html`) won**. It is an interactive schematic: hover, tap or Tab to a device to highlight its links and show a console readout.
7. Variant B was scrubbed for privacy (see below). Variant A was never pushed and exists only on the original PC.

## Privacy rules for the homelab diagram (public site)

Do not add any of these to anything in this repo:
- exact IP addresses, service port numbers, Tailscale tailnet name or ACL tags
- employer devices, where devices physically are
- backup schedules or backup gaps
- the OSINT container (CT 122) or any tool names on it

Cards show what each container does instead of its IP. Lines are labelled by purpose ("game port", "model API", "DNS over TLS").

## Open items, in rough order

1. **Single host wording.** The real homelab is one Proxmox host (GMKTec K12). `index.html` still says "multi-node Proxmox cluster" (homelab case study) and "Second Proxmox host / A second Proxmox VE node" (K12 tool card). Ask the owner before rewriting.
2. **Concepts use older text.** The `SITE` data in `concepts/*.html` was copied before the owner's later rewrite of `index.html`, which shortened the wording and reordered the sections to Case studies, Projects, Experience, Skills, Education. When promoting blueprint-b, copy the current `SITE` data from `index.html` into it.
3. **Mobile placement of the diagram.** On desktop the full-width diagram under the hero text works. On a 375px phone it is a 1270px-tall block that scrolls sideways, placed before Experience. Proposed fix: on narrow screens, hide it behind a "View homelab diagram" toggle. The owner has not answered yet.
4. **Promote blueprint-b to `index.html`.** Change `../assets/` paths to `assets/`, keep the head metadata and JSON-LD, show the diff, and only then commit.
5. **`assets/preview.png` is missing**, so link previews on LinkedIn and similar sites show no image.
6. The diagram will need a refresh once CT 124 (Linux lab) and CT 125 (AdGuard DNS) finish setup and CT 112 (Terraria) is built.

## Owner preferences

- No em dashes anywhere (content, comments, code).
- Show the diff and stop before committing unless told otherwise.
- Do not invent facts, metrics or credentials.
- Plain, simple wording (see the owner's recent `index.html` rewrite).
