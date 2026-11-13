# cjportfolio

Personal portfolio site. Single page, static, no build step.

Live at: https://luckyajinn.github.io/cjportfolio/

## Overview

One HTML file with inline CSS and JavaScript. No framework, no package manager,
and no build. The only outside request is the Geist and Instrument Serif fonts
from Google Fonts, and the page falls back to system fonts without them.
Screenshots load from `assets/`.

The design is dark by default with a light theme toggle. The resume is the
main button in the nav and the hero. It has an animated dot grid in the hero, a project gallery that expands on hover, case studies
that stack as you scroll, a diagram of my home server with paths you can play, and a
command menu on Ctrl K (or Cmd K) for jumping around the page.

## Structure

```
cjportfolio/
  index.html      the site: markup, styles, SITE data, and scripts
  assets/         favicons, screenshots, and the link preview image
  resume/         source page for the resume PDF
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

One project can have `featured: true` and a `highlights` list of three or so
short points. It gets the large block above the gallery and is left out of
the gallery itself. The screenshot in the hero is that same project, and is
set in the hero markup.

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

The diagram is drawn by the second script block in `index.html` from four
objects: `N` (each device, with the text shown when you point at it),
`LAYOUT` (box positions and line paths for the wide drawing and the tall one),
`REL` (what lights up when a device is selected), and `SC` (the paths you can
play, one captioned step at a time). To add a container, add it to `N`, give
it a box in both layouts, and list its links in `REL`. Under 861 pixels wide
the page uses the tall drawing, so the diagram never scrolls sideways.

The three numbers above the diagram and the "Read the diagram as text" list
are written in the markup, so update them when the lab changes. The diagram
is public, so keep these out of it:

- exact IP addresses, port numbers, the tailnet name, and ACL tags
- employer devices and where devices physically are
- backup schedules and backup gaps
- security tooling that is not meant to be public

## Social preview

Shared links show `assets/preview.png` (1200 by 630), a card with my name and
title in the same look as the hero.

## Resume

`SITE.resume` holds the path to the resume PDF,
`assets/christian-jones-resume.pdf`. The contact section links to it and it
appears in the command menu. If the value is empty, the contact section shows
a "PDF coming soon" card instead.

The source is `resume/resume.html`, one Letter page in the same look as the
site. To update the resume, edit that file, open it in a browser, and print
to PDF (Letter, no headers or footers, background graphics on), saving over
the file in `assets/`. Check that it is still one page. The file avoids CSS
`position` and `z-index` on purpose, because positioned elements are written
to the PDF out of order and scramble the text that applicant tracking systems
read.

The dark band behind the name is a flat image, `resume/header-bg.jpg`, not CSS
gradients. See-through gradients print as PDF transparency, and some PDF
previews drop the dark layer under them and show the band pink. Keep every
colour in `resume.html` solid: no `rgba()`, `opacity`, or blend modes.

In `SITE.credentials`, `active: true` marks something in progress and
`done: true` marks something completed.

## Deployment

Served by GitHub Pages from the `main` branch, root folder. A push to `main`
publishes within about a minute.

## Notes

- Browser support: current browsers. The page uses CSS Grid, `aspect-ratio`, and
  the `<dialog>` element, with a fallback when `showModal` is missing.
- Responsive down to phone width, keyboard navigable, and it honors reduced
  motion preferences.
- Without JavaScript the hero and contact links still show, plus a short summary.
