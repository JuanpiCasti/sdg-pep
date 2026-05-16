# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A **static study website** (HTML + CSS only, no JavaScript, no build step) that recaps the
book *Preparación y Evaluación de Proyectos* by Nassir Sapag Chain (6th ed., McGraw-Hill 2014),
chapter by chapter. It is study material for the UTN FRBA course "Sistemas de Gestión". All
content is in **Spanish**.

The authoritative source content is the PDF in the working directory:
`Preparación y Evaluación de Proyectos - Sapag Chain.PDF` (370 pages).

## Running / previewing

There is nothing to build or test. Open `index.html` directly in a browser (works offline,
`file://`). Every page links to the others via relative `<a>` hrefs.

To verify the site after edits, check that internal links resolve:

```bash
python3 - <<'EOF'
import os, re
base='.'
err=0
for root,_,files in os.walk(base):
    for fn in (f for f in files if f.endswith('.html')):
        p=os.path.join(root,fn); t=open(p,encoding='utf-8').read()
        for href,_ in re.findall(r'href="([^"#]+)(#[^"]*)?"', t):
            if href.startswith('http') or not href: continue
            if not os.path.exists(os.path.normpath(os.path.join(root,href))):
                print('BROKEN', fn, '->', href); err+=1
print('broken links:', err)
EOF
```

## Reading the source PDF

The repo's host has no PDF tooling. A pymupdf virtualenv was created for text extraction at
`/tmp/pdfenv` (may need recreating). To extract page text:

```bash
uv venv /tmp/pdfenv && uv pip install --python /tmp/pdfenv/bin/python pymupdf
/tmp/pdfenv/bin/python -c "import fitz; d=fitz.open('Preparación y Evaluación de Proyectos - Sapag Chain.PDF'); print(d[N].get_text())"
```

Page mapping: **book page N = PDF page N + 15** (Chapter 1 starts at PDF page 16).

## Structure

- `index.html` — landing page; the 19 chapters as cards grouped into 8 thematic blocks.
- `capitulos/capitulo-01.html` … `capitulo-19.html` — one file per chapter.
- `formulario.html` — consolidated reference of every formula.
- `glosario.html` — glossary of key terms, alphabetical.
- `assets/styles.css` — the single shared stylesheet.

## Chapter page template (must stay consistent across all 19 files)

Each `capitulo-NN.html` follows the same skeleton, in this order: `topbar` → `migas`
(breadcrumb) → `cap-cabecera` (block tag + number + title) → `caja--objetivo` (chapter
objective) → `cap-indice` (section index with `#` anchors) → numbered `<h2 id="sN">` sections
→ `resumen` → `preguntas` → `biblio` → bottom `cap-nav` (prev/next) → `pie`. Chapter pages use
`../assets/styles.css` and `../index.html`; root pages use `assets/styles.css` and `index.html`.

## Content conventions

- **Fidelity to the book**: prose paraphrases the source faithfully without losing depth;
  worked examples reproduce the book's actual numbers (cuadros/figuras).
- **Conceptual chapters** (1-4, 10, 11): clear and concise prose.
- **Quantitative chapters** (5-9, 12-19): every formula rendered, every example shown.
- **Formulas**: native **MathML** (`<math display="block">`), each followed by a `dl.vars`
  defining the variables. No KaTeX/MathJax — the site has no JavaScript.
- **Book tables (cuadros)**: rebuilt as native `<table>` inside `.tabla-wrap`.
- **Diagrams**: recreated with the HTML/CSS components below (not raster images).
- Worked examples go in `.ejemplo` blocks; the headline result in `.resultado`.

## CSS components (defined in `assets/styles.css`)

Reuse these classes rather than inventing new ones: `topbar`, `pagina`/`contenido`, `migas`,
`cap-cabecera`/`bloque-tag`, `caja` (+ `--objetivo` / `--idea` / `--nota`), `cap-indice`,
`formula` (+ `--destacada`), `dl.vars`, `tabla-wrap`/`table`, `ejemplo`/`resultado`, `figure`,
`esquema`/`nodo` (classification diagrams), `flujo-pasos`/`paso` (process steps),
`conceptos`/`concepto` (definition cards), `resumen`, `preguntas`, `biblio`, `cap-nav`,
`hero`/`bloque`/`grid-caps`/`cap-card` (landing page). Theme: light background, blue accent
`#1a4d8f`, defined in `:root` custom properties. There is a `@media print` block.
