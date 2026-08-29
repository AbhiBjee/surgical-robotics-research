# SurgTwin project page

Static project page for the liver tumour palpation and multi-material surgical digital twin work.
Served by GitHub Pages. No build step, no dependencies to install.

## Folder layout

Create exactly this structure. Paths inside `index.html` are relative, so a project site
served from a subfolder will resolve them correctly.

```
surgtwin/
├── index.html
├── README.md
└── static/
    ├── images/
    │   ├── favicon.ico
    │   ├── teaser.png
    │   ├── formulations.png
    │   ├── force_depth.png
    │   ├── pipeline.png
    │   ├── poster_settling.png
    │   └── poster_retraction.png
    ├── videos/
    │   ├── palpation_teaser.mp4
    │   ├── settling_comparison.mp4
    │   └── grasp_retraction.mp4
    └── pdfs/
        ├── surgtwin_paper.pdf
        └── intuitive_poster.pdf
```

## Publishing

```bash
cd surgtwin
git init
git add .
git commit -m "Initial project page"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/surgtwin.git
git push -u origin main
```

Then in the repository on GitHub, open Settings, then Pages. Set Source to
"Deploy from a branch", Branch to `main`, folder to `/ (root)`, and click Save.
The live URL appears at the top of that page after one to two minutes.

Live URL will be: `https://YOURUSERNAME.github.io/surgtwin/`

## Preparing media

Compress every video before committing. Target under 15 MB per clip.

```bash
ffmpeg -i raw_capture.mp4 -vcodec libx264 -crf 28 -preset slow \
       -vf "scale=1280:-2" -an -movflags +faststart output.mp4
```

Generate a poster frame so the video area is not blank while loading.

```bash
ffmpeg -i output.mp4 -ss 00:00:01 -vframes 1 -vf "scale=1280:-2" poster.png
```

Export figures from matplotlib or Plotly at twice the display size so they stay
sharp on high resolution screens.

```python
fig.savefig("static/images/force_depth.png", dpi=200, bbox_inches="tight")
```

## Limits to respect

- A single file on GitHub cannot exceed 100 MB.
- A published GitHub Pages site should stay under 1 GB in total.
- Do not commit raw simulation captures. Commit only the compressed versions.

## TODO checklist inside index.html

Search the file for the word TODO. There are nine of them.

1. Title and search engine metadata in the head block.
2. Favicon. Replace `static/images/favicon.ico` with your own.
3. Venue line, updated on acceptance.
4. Author links. Point each name at a real page or remove the link.
5. Link buttons. Delete any button whose target does not exist yet.
6. Abstract. Paste the final version from the paper.
7. Interface integrity table numbers.
8. Palpation figure and contrast table numbers.
9. BibTeX entry, updated with the real venue and DOI.

## Attribution

The layout follows the Nerfies project page, whose source is released under
Creative Commons Attribution ShareAlike 4.0. Keep the attribution line in the footer.
