# cjportfolio

Personal portfolio site. Single page, static, no build step required to run it.

Live at: https://luckyajinn.github.io/cjportfolio

## Overview

A self contained HTML page covering professional experience, selected IT
operations work, and the internal tooling built alongside it. Written as one
file with no framework and no runtime dependencies, so it can be served from
GitHub Pages, opened from a local path, or emailed as a single attachment.

## Structure

```
cjportfolio/
  index.html      the site, source of truth for content and styling
  build.py        optional, produces a standalone single file version
  assets/         screenshots referenced by the tooling section
```

## Editing content

All page content lives in a single `SITE` object at the top of the script block
in `index.html`. The markup is generated from it, so adding or changing an entry
means editing an array, not hand writing HTML.

| Array | Drives |
| --- | --- |
| `roles` | Experience section |
| `cases` | Selected work case studies |
| `tools` | Tooling gallery |
| `skills` | Technical scope columns |
| `credentials` | Education and certifications |

### Adding a tool

Append an object to `SITE.tools`:

```js
{
  name: "Tool name",
  blurb: "What problem it solves and how.",
  stack: ["HTML", "JavaScript"],
  shots: ["assets/tool-name-1.png", "assets/tool-name-2.png"],
  captions: ["First view", "Second view"]
}
```

Behavior follows the number of screenshots supplied:

- No entries in `shots`: the card renders a labeled placeholder.
- One entry: a static screenshot that opens in the viewer on click.
- Two or more: hovering the card cycles the images, and clicking opens a viewer
  with previous and next controls, a thumbnail strip, and arrow key navigation.

`captions` runs in the same order as `shots` and labels each frame in the viewer.
A screenshot listed in `shots` but missing from `assets/` falls back to the
placeholder rather than rendering as a broken image.

The cascade interval is set by the `CASCADE_MS` constant directly above the
`SITE` object. It is disabled automatically for visitors who have reduced motion
enabled in their operating system.

## Screenshots

Capture at roughly 1600 pixels wide. Cards crop to a 16:10 frame anchored to the
top of the image, and the full image is shown uncropped in the viewer.

Screenshots of internal tools are loaded with placeholder data before capture.
Nothing identifying an employer environment belongs in a public repository.

## Standalone build

`build.py` produces `index-standalone.html`, a single file with every asset
inlined as a base64 data URI and no external dependencies at all. Useful for
sending the page as an attachment or opening it without a server.

```
python build.py
```

Standard library only, no packages to install. The script reports each asset it
inlines, the final file size, and any path referenced in `index.html` that is
missing from disk.

Deploy `index.html` with the `assets` folder for hosting, since separate images
cache independently. Use the standalone build for offline or attachment use.

## Deployment

Served by GitHub Pages from the `main` branch, root folder. Any push to `main`
publishes within about a minute.

## Notes

- No frameworks, no package manager, no build required for the hosted version.
- Browser support: any current browser. Uses CSS Grid, `aspect-ratio`, and
  standard DOM APIs.
- Responsive down to mobile, keyboard navigable, honors reduced motion
  preferences.
