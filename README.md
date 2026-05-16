# Preparación y Evaluación de Proyectos — Sitio de estudio

Sitio web estático de estudio que resume, capítulo por capítulo, el libro
**_Preparación y Evaluación de Proyectos_** de Nassir Sapag Chain, Reinaldo Sapag
Chain y José Manuel Sapag Puelma (6.ª edición, McGraw-Hill, 2014).

Es material de apoyo para la asignatura **Sistemas de Gestión** de la UTN FRBA.
Todo el contenido está en español.

---

## Características

- **Estático y sin dependencias**: solo HTML y CSS. No usa JavaScript ni requiere
  ningún paso de compilación (_build_).
- **Funciona sin conexión**: se puede abrir directamente desde el sistema de
  archivos (`file://`). Las páginas se enlazan entre sí con rutas relativas.
- **19 capítulos** organizados en **8 bloques temáticos**, más un formulario y un
  glosario de consulta.
- **Fórmulas en MathML nativo** (sin KaTeX ni MathJax), tablas reconstruidas como
  `<table>` y diagramas recreados con componentes HTML/CSS.
- **Diseño editorial** con tipografías servidas desde Google Fonts.
- Incluye una hoja de estilos de impresión (`@media print`).
- No indexable por buscadores (`robots.txt` + meta `noindex`).

---

## Cómo verlo

No hay nada que compilar ni instalar. Basta con abrir `index.html` en un navegador.

### Opción A — abrir el archivo directamente

Hacé doble clic en `index.html` o arrastralo al navegador. Funciona con `file://`.

> Nota: las tipografías se cargan desde Google Fonts (CDN). Sin conexión a
> internet el contenido se ve igual, pero con tipografías de respaldo del sistema.

### Opción B — servidor local

Para una vista más fiel (y evitar restricciones de algunos navegadores con
`file://`), conviene servir la carpeta con un servidor HTTP simple:

```bash
python3 -m http.server 4321
```

Luego abrí <http://localhost:4321/index.html> en el navegador.

---

## Estructura del proyecto

```
.
├── index.html              Portada: 19 capítulos en 8 bloques temáticos
├── formulario.html         Todas las fórmulas del libro, reunidas
├── glosario.html           Glosario de términos clave, alfabético
├── robots.txt              Bloqueo de rastreo por buscadores
├── README.md               Este archivo
├── CLAUDE.md               Guía para asistentes de IA que trabajen en el repo
├── assets/
│   └── styles.css          Única hoja de estilos compartida por todo el sitio
├── capitulos/
│   ├── capitulo-01.html     ┐
│   ├── ...                  │  Un archivo por capítulo (01 a 19)
│   └── capitulo-19.html     ┘
└── Preparación y Evaluación de Proyectos - Sapag Chain.PDF
                            Fuente original del contenido (370 páginas)
```

Las páginas de la raíz (`index`, `formulario`, `glosario`) referencian
`assets/styles.css` e `index.html`; las páginas de capítulo usan rutas relativas
(`../assets/styles.css`, `../index.html`).

### Bloques temáticos

| Bloque | Tema                                         | Capítulos |
|:------:|----------------------------------------------|:---------:|
| 1      | Fundamentos                                  | 1–3       |
| 2      | El estudio de mercado                        | 4–5       |
| 3      | El estudio técnico                           | 6–9       |
| 4      | Estudio organizacional y legal               | 10–11     |
| 5      | Construcción de la información financiera    | 12–14     |
| 6      | Criterios de evaluación y decisión           | 15–16     |
| 7      | Riesgo y sensibilidad                        | 17–18     |
| 8      | Evaluación social                            | 19        |

---

## Plantilla de las páginas de capítulo

Las 19 páginas de capítulo siguen el mismo esqueleto, en este orden:

1. `topbar` — barra de navegación superior.
2. `migas` — ruta de navegación (_breadcrumb_).
3. `cap-cabecera` — etiqueta de bloque, número y título del capítulo.
4. `caja--objetivo` — objetivo del capítulo.
5. `cap-indice` — índice de secciones con anclas `#`.
6. Secciones numeradas `<h2 id="sN">`.
7. `resumen` — síntesis del capítulo.
8. `preguntas` — preguntas y problemas.
9. `biblio` — bibliografía.
10. `cap-nav` — navegación al capítulo anterior y siguiente.
11. `pie` — pie de página.

Mantener esta estructura consistente entre los 19 archivos es importante.

---

## Convenciones de contenido

- **Fidelidad al libro**: la prosa parafrasea la fuente sin perder profundidad;
  los ejemplos resueltos reproducen los números reales del libro (cuadros y figuras).
- **Capítulos conceptuales** (1–4, 10, 11): prosa clara y concisa.
- **Capítulos cuantitativos** (5–9, 12–19): toda fórmula renderizada y todo
  ejemplo desarrollado.
- **Fórmulas**: MathML nativo (`<math display="block">`), seguidas de una lista
  `dl.vars` que define las variables.
- **Cuadros del libro**: reconstruidos como `<table>` dentro de `.tabla-wrap`.
- **Diagramas**: recreados con componentes HTML/CSS, no como imágenes.
- Los ejemplos resueltos van en bloques `.ejemplo`, con el resultado destacado en
  `.resultado`.

---

## Diseño y estilos

Todo el sitio comparte una única hoja de estilos: **`assets/styles.css`**.

- **Tipografías** (Google Fonts):
  - _Source Serif 4_ — títulos y texto de lectura.
  - _Hanken Grotesk_ — interfaz: etiquetas, navegación, encabezados de tabla.
- **Paleta**: papel cálido de fondo, tinta negra cálida y un azul tinta de acento;
  definida con propiedades personalizadas (`--azul`, `--tinta`, `--fondo`, etc.)
  en el bloque `:root`.
- **Componentes CSS reutilizables**: `topbar`, `pagina`/`contenido`, `migas`,
  `cap-cabecera`/`bloque-tag`, `caja` (`--objetivo` / `--idea` / `--nota`),
  `cap-indice`, `formula` (`--destacada`), `dl.vars`, `tabla-wrap`/`table`,
  `ejemplo`/`resultado`, `figure`, `esquema`/`nodo`, `flujo-pasos`/`paso`,
  `conceptos`/`concepto`, `resumen`, `preguntas`, `biblio`, `cap-nav`,
  `hero`/`bloque`/`grid-caps`/`cap-card` (portada).
- Hay un bloque `@media print` para que el material se imprima limpio.

Al editar el sitio conviene reutilizar estas clases en lugar de inventar nuevas.

---

## Verificación de enlaces internos

El sitio no tiene tests, pero conviene comprobar que ningún enlace interno quede
roto tras una edición. Este script recorre todos los `.html` y reporta los enlaces
que no resuelven:

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
                print('ROTO', fn, '->', href); err+=1
print('enlaces rotos:', err)
EOF
```

El resultado esperado es `enlaces rotos: 0`.

---

## Fuente del contenido

El contenido se basa en el PDF incluido en el repositorio:
`Preparación y Evaluación de Proyectos - Sapag Chain.PDF` (370 páginas).

Correspondencia de páginas: **página del libro N = página del PDF N + 15**
(el capítulo 1 comienza en la página 16 del PDF).

---

## Indexación

El sitio está configurado para **no ser indexado** por buscadores:

- `robots.txt` con `Disallow: /` para todos los rastreadores.
- Etiqueta `<meta name="robots" content="noindex, nofollow">` en cada página.

---

## Licencia y uso

Material de uso **educativo** para la asignatura _Sistemas de Gestión_.
Todos los derechos del contenido original pertenecen a sus autores y a la
editorial McGraw-Hill.
