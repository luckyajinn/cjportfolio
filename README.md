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
  assets/         favicons and tool screenshots
  concepts/       earlier redesign mockups, kept for reference, not linked
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
  short: "One line for the card.",
  status: "Optional label",       // optional
  shots: ["assets/project-1.png", "assets/project-2.png"],
  captions: ["First view", "Second view"],
  sections: [
    { h: "What it does", p: "Short paragraph." },
    { h: "Features", list: ["Point one", "Point two"] }
  ],
  stack: ["HTML", "JavaScript"]
}
```

- Each project is a panel in the gallery. The first screenshot is the panel
  background, and `group` shows as a label.
- Hovering or focusing a panel expands it. On phones the panels stack.
- Clicking a panel opens a dialog with the write-up and a screenshot stage
  that has previous and next controls, a thumbnail strip, and arrow key
  navigation.
- Clicking the screenshot enlarges it inside the dialog.
- New projects also show up in the command menu.

Case studies use `kicker`, `title`, `problem`, `approach`, `outcome`, and
`stack`. A `summary` line is optional and only shows when present.

## Screenshots

Capture at roughly 1600 pixels wide and save to `assets/`. Cards crop to a
fixed frame anchored to the top of the image, and the viewer shows the full
image.

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

The page points its link preview at `assets/preview.png` (1200 by 630). That
file has not been added yet, so shared links show no image until it is.

## Deployment

Served by GitHub Pages from the `main` branch, root folder. A push to `main`
publishes within about a minute.

## Notes

- Browser support: current browsers. The page uses CSS Grid, `aspect-ratio`, and
  the `<dialog>` element, with a fallback when `showModal` is missing.
- Responsive down to phone width, keyboard navigable, and it honors reduced
  motion preferences.
- Without JavaScript the hero and contact links still show, plus a short summary.
