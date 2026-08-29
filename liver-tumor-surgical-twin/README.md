# Liver Tumor Surgical Twin — project page

Source for the project page accompanying **FEM Paradigms for Heterogeneity Modeling in
Multi-Material Tissue-Tumor Surgical Twins**.

**Live page:** https://abhibjee.github.io/surgical-robotics-research/liver-tumor-surgical-twin/

This folder contains presentation assets only. It holds no simulation code. The SOFA scenes,
the scene generation pipeline and the analysis scripts live in separate repositories and will
be released publicly once the paper is accepted.

---

## What is in here

```
liver-tumor-surgical-twin/
├── index.html                          the entire page, single self-contained file
├── README.md                           this file
└── static/
    ├── images/
    │   ├── scene_overview.png          hero, bimanual setup with stress-coloured mesh
    │   ├── palpation_progression.png   4x4 grid, deformation and stress at increasing depth
    │   ├── formulations.png            cutaway views of the three FEM formulations
    │   ├── pipeline.png                automated scene generation diagram
    │   ├── poster_stiffness.png        still frame for stiffness_variants.mp4
    │   ├── poster_ldl.png              still frame for palpation_ldl.mp4
    │   ├── poster_cg.png               still frame for palpation_cg.mp4
    │   └── poster_settling.png         still frame for settling_comparison.mp4
    └── videos/
        ├── stiffness_variants.mp4      palpation at 25, 55 and 75 kPa side by side
        ├── settling_comparison.mp4     gravitational settling, three formulations
        ├── palpation_ldl.mp4           direct solver, SparseLDL^T
        └── palpation_cg.mp4            iterative solver, conjugate gradient
```

`index.html` carries its own CSS in a `<style>` block and pulls Bulma, Font Awesome and
Academicons from a CDN. There is no build step and nothing to install. Open the file in a
browser to preview any change.

---

## Updating the page

Edit `index.html`, check it locally, then from the repository root:

```powershell
git add .
git commit -m "Describe the change"
git push
```

The live site rebuilds automatically about a minute after each push. GitHub Pages settings
never need revisiting.

---

## Media pipeline

Raw screen captures live outside the repository in `D:\GitHub_Website\raw_captures\` so they
can never be committed by accident. Every video in `static/videos/` is a compressed derivative.

Full-width clips (hero, settling):

```powershell
ffmpeg -i ..\..\raw_captures\INPUT.mp4 -vcodec libx264 -crf 30 -preset slow -vf "scale=1280:-2" -an -movflags +faststart static\videos\OUTPUT.mp4
```

Half-width clips (the two solver comparisons):

```powershell
ffmpeg -i ..\..\raw_captures\INPUT.mp4 -vcodec libx264 -crf 30 -preset slow -vf "scale=960:-2" -an -movflags +faststart static\videos\OUTPUT.mp4
```

Poster frame for any clip:

```powershell
ffmpeg -i static\videos\CLIP.mp4 -ss 00:00:02 -vframes 1 -vf "scale=1280:-2" static\images\poster_CLIP.png
```

Trim a clip that is too long:

```powershell
ffmpeg -i input.mp4 -t 12 -c copy output.mp4
```

Target roughly 3 to 15 MB per clip. Raise `-crf` for smaller files, lower it for better quality.
SOFA renders are mostly flat colour and compress very efficiently, so 30 is usually safe.

---

## Constraints that are easy to forget

**Filenames are case sensitive on GitHub Pages.** Windows treats `Static` and `static` as the
same folder; the live server does not. Every path in this project is lowercase. A capitalised
folder produces a page that looks perfect locally and shows broken images once deployed.

**All asset paths are relative.** Written as `static/images/x.png`, never `/static/images/x.png`.
A leading slash resolves to the root of `abhibjee.github.io` and silently fails, because this is
a project site served from a subfolder.

**File size limits.** GitHub warns above 50 MB and rejects above 100 MB. Never commit a raw
capture. If a push is rejected for file size, the offending file is in the commit history and
must be removed from history, not merely from disk.

**Git LFS will not work here.** GitHub Pages does not serve LFS-backed files; a visitor receives
the pointer text rather than the video. If a clip genuinely cannot be compressed under the limit,
host it unlisted on YouTube and embed it instead.

---

## Outstanding items

- [ ] **Correct the Poisson ratio labels in `stiffness_variants.mp4`.** The burned-in overlay
      currently reads 4.5 and 3.5; these should be 0.45 and 0.35. A ratio above 0.5 is
      thermodynamically impossible for an isotropic material and will be noticed immediately.
- [ ] Add `static/images/favicon.ico`. Referenced in the head block, currently missing.
- [ ] Add `static/pdfs/` with the paper and the Intuitive poster, or convert those two buttons
      to the dashed pending style already used for Code.
- [ ] Check whether the von Mises colour scale in `palpation_progression.png` is normalised
      per frame. If it is, either fix the range so all four columns share one scale, or state
      the per-frame normalisation in the caption.
- [ ] Confirm the contact force at the 35 mm column is nonzero. If that frame is a tunnelling
      artifact rather than a genuine indentation, drop it and show three columns.
- [ ] Replace the placeholder author links, which currently all point at `#`.
- [ ] Add the arXiv link once a preprint is posted, and remove the button until then.

---

## Citation

```bibtex
@inproceedings{bhattacharjee2026surgtwin,
  title     = {FEM Paradigms for Heterogeneity Modeling in Multi-Material
               Tissue-Tumor Surgical Twins},
  author    = {Bhattacharjee, Abhinaba and Shaikh, Noah and
               Stefanidis, Dimitrios and Qureshi, Ahmed and Anwar, Sohel},
  booktitle = {IEEE/RSJ International Conference on Intelligent Robots and
               Systems (IROS) Workshop on Surgical Digital Twins},
  year      = {2026}
}
```

---

## Attribution

The page layout follows the [Nerfies](https://nerfies.github.io) project page, whose source is
released under a [Creative Commons Attribution ShareAlike 4.0 International
License](http://creativecommons.org/licenses/by-sa/4.0/). The attribution line in the page footer
should be kept.
