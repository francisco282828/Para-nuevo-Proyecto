# Artefactos

Repositorio dedicado a alojar mis artefactos: páginas y herramientas HTML
autocontenidas, sin build ni dependencias externas.

## Estructura

```
.
├── index.html          # galería que lista todos los artefactos
└── artefactos/
    └── <slug>/
        └── index.html  # un artefacto
```

## Añadir un artefacto nuevo

1. Crea la carpeta `artefactos/<slug>/` y coloca dentro su `index.html`.
2. Registra la entrada en el array `ARTEFACTOS` del `index.html` de la raíz:

```js
{
  slug: "mi-artefacto",
  titulo: "Mi artefacto",
  descripcion: "Una frase sobre qué hace.",
  etiqueta: "referencia",
  fecha: "2026-07"
}
```

La galería se genera desde ese array, así que basta con esas dos cosas.

## Convenciones

- **Autocontenido**: todo el CSS y el JS van en línea. Evita CDNs — así el
  artefacto funciona offline, abierto directamente desde el disco.
- **Un artefacto, una carpeta**, siempre con `index.html` como punto de entrada.
- **Tema visual compartido**: fondo oscuro morado con acentos dorados y
  turquesa. Las variables CSS están al inicio de cada archivo.
- **Responsive y accesible**: sin scroll horizontal, y respeta
  `prefers-reduced-motion`.

## Ver los artefactos

Abre `index.html` en el navegador. Si en algún momento activas GitHub Pages
(Settings → Pages → rama y carpeta `/`), la galería queda publicada en la raíz
del sitio.

## Artefactos actuales

| Artefacto | Descripción |
| --- | --- |
| [Comandos de Claude Code](artefactos/comandos-claude-code/) | Catálogo interactivo de los 58 comandos slash, con buscador y filtros por categoría. |
