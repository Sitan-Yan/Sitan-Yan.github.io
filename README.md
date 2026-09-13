# Sitan Yan — Academic Website

Personal academic homepage featuring research in robotics, precision engineering, and 3D vision. Built with plain HTML, CSS, and JavaScript; no build step is required.

## Structure

- `index.html`: homepage, education, research, publications, and awards.
- `css/main.css`: shared base styles; `css/homepage.css`: homepage layout.
- `css/project.css`: shared project layout; `css/cfrp.css` and `css/focus-constrained.css`: page-specific styles.
- `projects/`: SFF, focus-constrained visual control, and CFRP machining pages, each with its own `assets/`.
- `assets/images/`: homepage-only images, including the dynamic obstacle avoidance preview.
- `files/Sitan_Yan_CV.pdf`: public CV with direct contact details removed and unpublished method details summarized.
- `js/script.js`: small homepage behavior such as the copyright year.

Dynamic Obstacle Avoidance currently has a homepage entry only. Its preview is a frame from `multiple_obstacles.mp4`; no project page is linked.

## Preview locally

Run from the repository root:

```sh
python -m http.server 8765 --bind 127.0.0.1
```

Open [the local homepage](http://127.0.0.1:8765/). The same static files can be served by GitHub Pages.

## Editing

Use UTF-8, two-space indentation, and the included `.prettierrc.json` (100-column target). Keep shared styles in `css/` and media under the relevant `assets/` directory. With Prettier installed:

```sh
prettier --write index.html "projects/*/index.html" "css/*.css" "js/*.js"
```

After editing, check desktop and mobile layouts, image/video loading, project links, and the CV link. Project media/source notes are kept in `IMAGE_GUIDE.md` or `MEDIA_GUIDE.md` alongside each project page.

Only publish sanitized CV and media copies. Keep original documents and unpublished formulas, implementation details, and private contact data outside this repository.
