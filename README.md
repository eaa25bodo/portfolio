# Bogi Dobos Portfolio

A multimedia design portfolio built with plain HTML, CSS, and JavaScript. The site uses a flower-based home screen as its navigation and showcases visual, UX, illustration, and branding work.

## Run locally

Install the dependencies:

```bash
npm install
```

Start the Vite development server:

```bash
npm run dev
```

Create a production build:

```bash
npm run build
```

Preview the production build:

```bash
npm run preview
```

## Project structure

- `index.html` — the portfolio page and its navigation logic
- `styles.css` — shared design tokens and component styles
- `assets/` — portfolio imagery used by the page
- `package.json` — Vite scripts and development dependency
- `vite.config.js` — Vite configuration

## Notes

This repository is intentionally kept as a static HTML portfolio. The React/Vite export described in earlier design files is not enabled because the required React source files are not part of the repository. Keeping the existing HTML entry point avoids replacing the working site with an incomplete React shell.

Do not commit generated `dist/` output or local dependency folders such as `node_modules/`.
