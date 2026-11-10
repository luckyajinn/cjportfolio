# cjportfolio

Personal portfolio site. Single page, static, no build step.

Live at: https://luckyajinn.github.io/cjportfolio/

## Overview

One HTML file with inline CSS and JavaScript. No framework, no package manager,
and no build. The only outside request is the Geist and Instrument Serif fonts
from Google Fonts, and the page falls back to system fonts without them.
Screenshots load from `assets/`.

The design is dark by default with a light theme toggle. It has an animated
dot grid in the hero, a project gallery that expands on hover, case studies
that stack as you scroll, an interactive diagram of my home server, and a
command menu on Ctrl K (or Cmd K) for jumping around the page.

## Structure

```
cjportfolio/
  index.html      the site: markup, styles, SITE data, and scripts
  assets/         favicons, screenshots, and the link preview image
  concepts/       earlier designs, kept for reference, not linked
                  (blueprint-live.html is the page that was live until November 2026)
```

## Editing content

Experience, case studies, projects, skills, and credentials live in the `SITE`
object at the top of the main script block in `index.html`. The page builds
those sections from it. The hero text, the statement and numbers under it,
section intros, contact links, footer, and the homelab diagram are written
directly in the markup.

| Array | Drives |
| --- | --- |
| `roles` | Experience |
| `cases` | Case studies |
| `tools` | Projects |
| `skills` | Skills |
| `credentials` | Education and certifications |

### Adding a project

Append an object to `SITE.tools`:

```js
{
  name: "Project name",
  group: "professional",          // or "personal"
  short: "One line for the panel.",
  status: "Optional label",       // optional
  shots: ["assets/project-1.webp", "assets/project-2.webp"],
  captions: ["First view", "Second view"],
  sections: [
    { h: "What it does", p: "Short paragraph." },
    { h: "Features", list: ["Point one", "Point two"] }
  ],
  links: [{ label: "repo-name", url: "https://github.com/..." }],  // optional
  stack: ["HTML", "JavaScript"]
}
```

Each `group` gets a tab above the gallery (Professional, Personal), in the
order the groups first appear in the list. The first project in each group is
the one shown open, so put the strongest one first.

- Each project is a panel in the gallery. The first screenshot is the panel
  background.
- Hovering or focusing a panel expands it. On phones the panels stack.
- Clicking a panel opens a dialog with the write-up and a screenshot stage
  that has previous and next controls, a thumbnail strip, and arrow key
  navigation.
- Clicking the screenshot enlarges it inside the dialog.
- New projects also show up in the command menu.

Case studies use `kicker`, `title`, `problem`, `approach`, `outcome`, and
`stack`. A `summary` line is optional and only shows when present.

## Screenshots

Capture at roughly 1600 pixels wide. Panels crop to a fixed frame anchored to
the top of the image, and the write-up shows the full image. Tall screenshots
fill the frame from the top, and clicking one opens it full size.

The page uses WebP copies in two widths: `name.webp` (up to 1600 pixels wide)
and `name-800.webp` (800 pixels wide, used for panels and thumbnails). List
only `assets/name.webp` in `shots`; the page works out the small one. To add
a screenshot, save both sizes. With Python and Pillow:

```python
from PIL import Image
im = Image.open("shot.png").convert("RGB")
for width, suffix, quality in [(1600, "", 86), (800, "-800", 80)]:
    out = im if im.width <= width else im.resize((width, round(im.height * width / im.width)))
    out.save(f"assets/name{suffix}.webp", quality=quality, method=6)
```

A plain PNG or JPG path in `shots` also works. It just loads the one file.
The PNG screenshots still in `assets/` are the originals of the older
screenshots, and only the archived pages in `concepts/` load them.

Screenshots of internal tools are loaded with placeholder data before capture.
Nothing identifying an employer environment belongs in a public repository.

## The homelab diagram

The diagram is inline SVG with a `DATA` object in the second script block
that drives the hover, focus, and tap readout. Hover, tap, or tab to a device to
highlight its links. The diagram is public, so keep these out of it:

- exact IP addresses, port numbers, the tailnet name, and ACL tags
- employer devices and where devices physically are
- backup schedules and backup gaps
- security tooling that is not meant to be public

The interface labels (Gi0/x, VLAN tags, 10.0.x.x) are decoration, not real
addresses.

## Social preview

Shared links show `assets/preview.png` (1200 by 630), a card with my name and
title in the same look as the hero.

## Resume

`SITE.resume` holds the path to the resume PDF, for example
`"assets/christian-jones-resume.pdf"`. While it is empty the contact section
shows a "PDF coming soon" card. Once it is set, the card becomes a link and
the resume appears in the command menu.

## Deployment

Served by GitHub Pages from the `main` branch, root folder. A push to `main`
publishes within about a minute.

## Notes

- Browser support: current browsers. The page uses CSS Grid, `aspect-ratio`, and
  the `<dialog>` element, with a fallback when `showModal` is missing.
- Responsive down to phone width, keyboard navigable, and it honors reduced
  motion preferences.
- Without JavaScript the hero and contact links still show, plus a short summary.
