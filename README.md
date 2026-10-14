# Despliegue de Aplicaciones Web 2026-2027

Web de portafolio (Zensical) con las prácticas del módulo DPL de 2º DAM:
<https://alejandrodongar.github.io/Despliegue-de-Aplicaciones-2026---2027/>

## Organización

Este repositorio **no** contiene las entregas: cada práctica es un microproyecto
independiente con su propio `git init` y su propio repositorio remoto. Aquí solo
va un resumen de cada una con el enlace a su repositorio.

```text
2ºDAM/
├── Despliegue-de-Aplicaciones-2026---2027/   ← esta web (portafolio)
├── ut01-guiada-01/                           ← entrega: repo propio
├── ut02-guiada-04/                           ← entrega: repo propio
└── ...
```

## Añadir una práctica

1. Crear la entrega en su carpeta hermana (`git init`, un commit por paso, README, remoto).
2. Crear `docs/utX/<nombre>.md` con el resumen y el enlace al repositorio de la entrega.
3. Añadir su línea al `nav` de `zensical.toml` y a la tabla de `docs/utX/index.md`.

## Ver la web en local

```bash
uv run zensical serve
```

Al hacer push a `main`, el workflow `.github/workflows/docs.yml` la publica en GitHub Pages.
