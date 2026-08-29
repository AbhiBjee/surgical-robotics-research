# Semi-Automated Motion Primitives — project page

Source for the project page accompanying **Semi-Automated Motion Primitives for Force-aware
Rigid and Deformable Manipulation in Simulated Robotic Surgery**.

**Live page:** https://abhibjee.github.io/surgical-robotics-research/motion-primitives/

Presentation assets only. No simulation code lives here. The primitive layer, the Grasp-Lock
implementation and the evaluation harness are in a separate repository and will be released
once the paper is accepted.

---

## Contents

```
motion-primitives/
├── index.html                        the whole page, single self-contained file
├── README.md                         this file
└── static/
    ├── images/
    │   ├── scene_overview.png        hero, dVRK scene on the FRS-style board   (Fig. 1a)
    │   ├── architecture.png          five-layer block diagram                  (Fig. 1b)
    │   ├── tasks.png                 motion overlays of the six tasks          (Fig. 2)
    │   └── poster_randomization.png  still frame for the teaser video
    └── videos/
        └── domain_randomization.mp4  successive randomized episodes
```

Missing and referenced in `index.html`: `static/images/favicon.ico`. Until it exists the
browser tab shows a blank icon, which is cosmetic only.

`index.html` carries its own CSS in a `<style>` block and loads Bulma, Font Awesome and
Academicons from a CDN. There is no build step and nothing to install. To preview, run a local
server from the repository root rather than double clicking the file:

```powershell
python -m http.server 8000
```

Then open `http://localhost:8000/motion-primitives/`. A real server is needed because folder
links and relative paths behave differently under `file://` than they do on GitHub Pages.

---

## Page structure

| Section | Content |
|---|---|
| Hero | Title, authors, three buttons: Paper (pending), Reference video (Google Drive), Code (pending) |
| Teaser | `scene_overview.png` full width, then `domain_randomization.mp4` below it |
| Abstract | Three paragraphs condensed from the paper abstract |
| Layered architecture | Five-row layer stack, then `architecture.png` |
| Grasp-Lock | Four paragraphs: admittance-controlled closure, rigid vs deformable capture, instrument-derived radius, contact force caveat |
| Results | `tasks.png`, then Table I (isolated) and Table II (cascaded), deformable rows shaded gold |
| Cite | BibTeX with a copy button |

A back link to the research hub sits above the hero.

---

## Media

Raw captures stay outside the repository, in `D:\GitHub_Website\raw_captures\`, so they can
never be committed by accident.

Compress a full-width clip:

```powershell
ffmpeg -i ..\..\raw_captures\INPUT.mp4 -vcodec libx264 -crf 32 -preset slow -vf "scale=1280:-2" -an -movflags +faststart tmp_out.mp4
move -Force tmp_out.mp4 static\videos\OUTPUT.mp4
```

ffmpeg cannot read and write the same file in one command, hence the temporary name.

Trim a clip that runs long. `-c copy` re-encodes nothing, so this is instant and lossless:

```powershell
ffmpeg -i static\videos\domain_randomization.mp4 -t 20 -c copy tmp_dr.mp4
move -Force tmp_dr.mp4 static\videos\domain_randomization.mp4
```

Regenerate the poster frame after any trim, since the old timestamp may no longer exist:

```powershell
ffmpeg -y -i static\videos\domain_randomization.mp4 -ss 00:00:02 -vframes 1 -vf "scale=1280:-2" static\images\poster_randomization.png
```

Resize an oversized figure:

```powershell
ffmpeg -i static\images\FIGURE.png -vf "scale=1400:-1" tmp\FIGURE.png
```

Use 1600 rather than 1400 for dense multi-panel figures with small labels. The page container
is 960 px wide at most, so anything beyond about 1600 px is detail no screen will show.

**Targets.** Videos 3 to 12 MB. Figures under 1 MB. Whole `static` folder under 20 MB.
GitHub warns above 50 MB per file and rejects above 100 MB, and Pages does not serve Git LFS
files, so compression is the only route for large media.

---

## Deploying

This folder is a sibling of `liver-tumor-surgical-twin` inside `surgical-robotics-research`.
Pages is already switched on for that repository, so nothing needs configuring:

```powershell
git add .
git commit -m "Describe the change"
git push
```

Live about a minute later. The hub entry linking here is already in place at the repository
root `index.html`.

---

## Constraints that are easy to forget

**Case sensitivity.** Windows treats `Static` and `static` as the same folder; GitHub Pages
does not. Every path here is lowercase. A capitalised folder gives a page that looks perfect
locally and shows broken images once deployed. This has already cost one debugging session.

**Relative paths only.** `static/images/x.png`, never `/static/images/x.png`. A leading slash
resolves to the root of `abhibjee.github.io` and fails silently, because this is a project site
served from a subfolder.

**Link to `index.html`, not to the folder.** `../index.html` works both from disk and when
served. `../` only works when served.

**CSS belongs inside the `<style>` block.** Anything pasted after `</style>` renders as visible
text at the top of the page. If raw code appears on the rendered page, that is the cause.

---

## Outstanding items

- [ ] Add `static/images/favicon.ico`.
- [ ] Confirm the Google Drive reference video is set to "Anyone with the link". A restricted
      link on a public page shows a request-access screen, which is worse than no link.
- [ ] Update the venue eyebrow and the BibTeX entry on acceptance.
- [ ] Swap the Paper button from pending to a real link once the PDF can be shared.
- [ ] Point the author names at real pages, or leave them as plain text. Do not leave `href="#"`.
- [ ] Check that `domain_randomization.mp4` visibly shows several different starting
      configurations. If it reads as one long episode, the randomization claim is invisible and
      the clip is just a demo.

---

## Citation

```bibtex
@inproceedings{bhattacharjee2026primitives,
  title     = {Semi-Automated Motion Primitives for Force-aware Rigid and
               Deformable Manipulation in Simulated Robotic Surgery},
  author    = {Bhattacharjee, Abhinaba and Shaikh, Noah and
               Stefanidis, Dimitrios and Qureshi, Ahmed H. and Anwar, Sohel},
  booktitle = {IEEE/RSJ International Conference on Intelligent Robots and
               Systems (IROS) Workshop on Embodied Learning for Surgical Robotics},
  year      = {2026}
}
```

---

## Attribution

Layout follows the [Nerfies](https://nerfies.github.io) project page, released under
[CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/). Keep the footer attribution.
