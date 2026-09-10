# Dickson Carrillo — Portafolio con Docker + Nginx

Portafolio personal estático (HTML + CSS) servido con Nginx en un contenedor Docker ligero. Proyecto de demostración de contenerización y despliegue de sitios estáticos.

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=flat&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=flat&logo=nginx&logoColor=white)
![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=flat&logo=html5&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## Problema que resuelve

Mostrar un portafolio personal desplegable de forma reproducible en cualquier máquina con Docker, sin depender de configuración local de servidores web.

## Características

- Página estática responsive en un solo `index.html` (sin build step)
- Imagen basada en `nginx:alpine` (~ligera)
- Despliegue con un comando vía Docker Compose (`8080:80`)
- Reinicio automático (`unless-stopped`)

## Stack tecnológico

| Capa | Tecnología |
|------|------------|
| Frontend | HTML5 + CSS puro (sin framework) |
| Servidor | Nginx (imagen `nginx:alpine`) |
| Contenedores | Docker + Docker Compose |
| Control de versiones | Git + GitHub |

## Arquitectura

```text
navegador → localhost:8080 → contenedor web (nginx:alpine:80) → /usr/share/nginx/html/index.html
```

- `Dockerfile`: copia `index.html` a la raíz web de Nginx y expone el puerto 80.
- `docker-compose.yml`: construye la imagen local y mapea `8080:80`.

## Instalación

Requisitos: Docker y Docker Compose.

```bash
git clone https://github.com/dacc-code/dickson-cv.git
cd dickson-cv
docker compose up --build
```

## Uso

Abrir en el navegador:

```text
http://localhost:8080
```

Detener:

```bash
docker compose down
```

## Screenshots / Demo

- TODO: agregar captura de pantalla (`docs/screenshot.png`).
- TODO: agregar URL de demo pública si existe.

## API

No aplica — sitio 100% estático, sin backend ni endpoints.

## Testing

No hay suite de tests actualmente.

Verificación manual mínima:

```bash
docker compose up --build -d
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080
# esperado: 200
docker compose down
```

- TODO: agregar chequeo automatizado (ej. workflow de GitHub Actions que construya la imagen y verifique HTTP 200).

## Deployment

Local con Docker Compose. Para producción servir la misma imagen detrás de un reverse proxy con TLS.

```bash
docker build -t dickson-cv .
docker run --rm -p 8080:80 dickson-cv
```

## Seguridad

- Imagen base `nginx:alpine` (superficie reducida).
- Sin secretos, sin backend, sin dependencias de runtime.
- No commitear archivos `.env` ni credenciales (ver `.gitignore`).

## Calidad del código

- Un solo archivo HTML autocontenido (CSS inline).
- Convención: cambios en rama feature/fix, PR hacia `main`, sin push directo a `main`.

## Licencia

MIT — ver [LICENSE](LICENSE).
