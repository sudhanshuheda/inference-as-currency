# Inference as Currency — site

A self-contained, Gwern-styled static site for *Inference as Currency: A Monetary Theory of the AI Cycle*. The site and the downloadable PDF contain the same content — a tight ~1,300-word / 6-minute essay that fits in three printed pages.

## Files

| File | Purpose |
|---|---|
| `index.html` | The essay. Single self-contained HTML — embedded CSS, MathJax via CDN, vanilla JS. No build step. |
| `thesis_3page.pdf` | Print-ready PDF of the same essay. Linked from the top bar. |
| `README.md` | This file. |

## Local preview

```bash
# Any static server works:
python3 -m http.server 8000        # then visit http://localhost:8000
# or
npx serve .                         # if you have Node
```

Or just double-click `index.html` to open in a browser. MathJax loads from CDN over `file://`.

## Deployment

Drop the `site/` folder on any static host. No build step.

```bash
# GitHub Pages
cd site/ && git init && git add . && git commit -m "initial"
git remote add origin git@github.com:USER/inference-as-currency.git
git push -u origin main
# Enable Pages in repo settings (branch=main, folder=/)

# Netlify drop
# Drag the site/ folder onto https://app.netlify.com/drop

# Vercel
cd site/ && npx vercel

# Cloudflare Pages
# Push to GitHub, connect repo. No build command. Output dir = /
```

## Design notes

- **Typography.** Source Serif 4 for body, Source Sans 3 for headings, IBM Plex Mono for code. All from Google Fonts.
- **Background.** Warm off-white (`#fbf8f1`) in light mode; warm dark (`#1a1611`) in dark mode.
- **Layout.** Floating TOC on the left, body in the middle. Single column on screens ≤ 900px.
- **Drop cap.** First letter of the lead paragraph set in the accent colour at 3.6× body size.
- **Math.** Display math (`$$...$$`) and inline math (`$...$`) rendered client-side via MathJax 3.
- **Dark mode.** Toggle in the top bar. Persists via `localStorage`. Respects `prefers-color-scheme` on first load.
- **Active section highlight.** TOC entries highlight as you scroll.

## Customising

Everything is in `index.html`. The CSS is in a single `<style>` block; the JS for theme + TOC highlighting is in a `<script>` block at the bottom.

To change the colour palette, edit the `:root` and `[data-theme="dark"]` CSS variables.

To regenerate the body from a markdown source:

```bash
pandoc thesis_3page.md \
  -f markdown+tex_math_dollars+footnotes \
  -t html5 --section-divs --mathjax --syntax-highlighting=none \
  -o thesis_body.html
```

Then drop the body into the `<article class="content">` block inside `index.html`.

## Regenerating the PDF

The PDF is produced from the same markdown source via Chrome headless with a print stylesheet:

```bash
# Use the print-styled HTML wrapper in the project root, or build your own.
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless=new --disable-gpu \
  --no-pdf-header-footer \
  --virtual-time-budget=4000 \
  --print-to-pdf=thesis_3page.pdf \
  file:///path/to/styled.html
```
