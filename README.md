# HLS Live Converter — versión estática

Un único archivo `index.html`. Sin backend, sin Node, sin subir el video a
ningún servidor: todo corre en el navegador con **FFmpeg.wasm**.

## Cómo usarlo

No abras el archivo con doble clic (`file://`) — los navegadores bloquean la
carga del WASM por CORS. Sírvelo con cualquier servidor estático:

```bash
# Opción 1: Python
python3 -m http.server 8080

# Opción 2: Node
npx serve .

# Opción 3: PHP
php -S localhost:8080
```

Abre `http://localhost:8080` y listo.

## Despliegue

Al ser un solo archivo estático, se sube tal cual a GitHub Pages, Netlify,
Vercel, S3, Cloudflare Pages, o cualquier hosting estático — no requiere
configuración de servidor ni variables de entorno.

## Notas

- Usa `@ffmpeg/ffmpeg@0.11` (single-thread), por eso NO necesita headers
  `Cross-Origin-Opener-Policy` / `Cross-Origin-Embedder-Policy`, a diferencia
  de las versiones más nuevas multi-hilo de ffmpeg.wasm.
- El perfil 1080p es notablemente más lento que en un servidor real, porque
  todo el trabajo de `libx264` corre en WebAssembly dentro del navegador.
- El archivo generado incluye `stream.m3u8` + `segmento_XXX.ts`, empaquetados
  en un `.zip` con JSZip, listos para subir a tu CDN/servidor de streaming.
