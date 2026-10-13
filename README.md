[![Join our Discord!](https://img.shields.io/static/v1?message=join%20chat&color=9cf&logo=discord&label=discord)](https://discord.gg/sKeNQX4Wtj)
[![Netlify Status](https://api.netlify.com/api/v1/badges/27fa023d-7c73-4a3f-9791-b3b657a47100/deploy-status)](https://app.netlify.com/sites/mermaidjs/deploys)

# Mermaid Live Editor

Edit, preview and share mermaid charts/diagrams.

## Features

- Edit and preview flowcharts, sequence diagrams, gantt diagrams in real time.
- Save the result as a svg
- Get a link to a viewer of the diagram so that you can share it with others.
- Get a link to edit the diagram so that someone else can tweak it and send a new link back

## Live demo

You can try out a [live version](https://mermaid.live/).

## Galería de Ejemplos Tecnasa

Este fork incluye una galería de ejemplos de procesos empresariales para el curso de Microsoft Copilot.

### Acceder a los ejemplos

Visita `/ejemplos` o usa el menú principal (icono de hamburguesa) → "Ejemplos Tecnasa"

### Agregar nuevos ejemplos

Los ejemplos se encuentran en la carpeta `examples/tecnasa/`. Para agregar un nuevo ejemplo:

1. Crea un archivo `.mmd` con tu diagrama Mermaid en `examples/tecnasa/`
2. Actualiza `src/routes/(app)/ejemplos/+page.svelte` para incluirlo en la galería:
   - Agrega un nuevo objeto en el array `examples` con los campos: `id`, `title`, `description`, `filename`, y `code`
   - El campo `code` debe contener el contenido completo del diagrama

Ejemplo:

```typescript
{
  id: 'mi-proceso',
  title: 'Mi Proceso',
  description: 'Descripción del proceso',
  filename: 'mi-proceso.mmd',
  code: `flowchart TD
    A[Inicio] --> B[Fin]`
}
```

## Despliegue en Vercel

Este proyecto está configurado para desplegarse en Vercel usando `@sveltejs/adapter-static`.

### Configuración

1. Conecta tu repositorio de GitHub a Vercel
2. Vercel detectará automáticamente la configuración en `vercel.json`
3. Las variables de entorno en `.env` desactivan analytics y enlaces externos por defecto

### Variables de entorno en Vercel (opcional)

En la configuración del proyecto en Vercel, puedes agregar:

- `MERMAID_ANALYTICS_URL` - URL de tu instancia de Plausible (vacío por defecto)
- `MERMAID_DOMAIN` - Tu dominio para analytics
- `MERMAID_IS_ENABLED_MERMAID_CHART_LINKS` - `false` para desactivar enlaces a Mermaid Chart (recomendado)

### Build local

```sh
pnpm build
```

El sitio estático se generará en la carpeta `docs/`

# Contributors are welcome!

If you want to speed up the progress for mermaid-live-editor, join the Discord channel and contact knsv.

## Docker

### Run published image

```bash
docker run --platform linux/amd64 --publish 8000:8080 ghcr.io/mermaid-js/mermaid-live-editor
```

The published docker image is built using our default environment variables. You cannot override them when running the image. If you need to customize them, you will need to build the image yourself.

### To configure renderer URL

When building set the MERMAID_RENDERER_URL build argument to the rendering
service.
Example:
Default is`https://mermaid.ink`.
Set to empty string to disable PNG and SVG links under Actions

### To configure Kroki Instance URL

When building set the MERMAID_KROKI_RENDERER_URL build argument to your Kroki
instance.
Default is `https://kroki.io`
Set to empty string to disable Kroki link under Actions

### To configure Analytics

When building set the MERMAID_ANALYTICS_URL build argument to your plausible instance, and MERMAID_DOMAIN to your domain.

Default is empty, disabling analytics.

### To enable Mermaid Chart links and promotion

When building set the MERMAID_IS_ENABLED_MERMAID_CHART_LINKS build argument to `true`

Default is empty, disabling button to save to Mermaid Chart and promotional banner.

### To update the Security modal

The modal shown on clicking the security link assumes analytics, renderer, Kroki
and Mermaid chart are enabled. You can update it by modifying `Privacy.svelte`
if you wish.

### Development

```bash
docker compose up --build
```

Then open http://localhost:3000

### Building and running images locally

#### Build

```bash
docker build -t mermaid-js/mermaid-live-editor .
```

#### Run

```bash
docker run --detach --name mermaid-live-editor --publish 8080:8080 mermaid-js/mermaid-live-editor
```

Visit: <http://localhost:8080>

#### Stop

```bash
docker stop mermaid-live-editor
```

## Setup

Below link will help you making a copy of the repository in your local system.

https://docs.github.com/en/get-started/quickstart/fork-a-repo

## Requirements

- [Node.js](https://nodejs.org/en/) current LTS version
- [pnpm](https://pnpm.io/) package manager. Install with `corepack enable pnpm`

## Development

```sh
pnpm install
pnpm dev -- --open
```

This app is created with Svelte Kit.

## Release

When a PR is created targeting master, it will be built and deployed by Netlify.
The URL will be indicated in a Comment in the PR.

Once the PR is merged, it will automatically be released.
