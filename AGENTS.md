# AGENTS.md — Instrucciones para agentes de IA (dickson-cv)

> Leer este archivo antes de modificar el repo. Rama de trabajo obligatoria: nunca commitear directo a `main`.

## Descripción del proyecto

Portafolio estático de una sola página (`index.html`) servido por Nginx en Docker. Sin framework, sin backend, sin base de datos, sin autenticación, sin tests automatizados. Objetivo: demo reproducible de `Docker + Nginx` para reclutadores.

## Arquitectura

- `index.html` → única página (HTML + CSS inline, `lang="es"`).
- `Dockerfile` (`nginx:alpine`, `COPY index.html`, `EXPOSE 80`).
- `docker-compose.yml` (servicio `web`, build local, `8080:80`, `restart: unless-stopped`).

## Comandos principales

```bash
docker compose up --build        # servir en http://localhost:8080
docker compose down              # detener
docker build -t dickson-cv .     # build manual
docker run --rm -p 8080:80 dickson-cv
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080  # esperado 200
```

No hay package manager, linter ni test runner configurados.

## Estructura

```text
.
├── index.html          # toda la UI (único archivo frontend)
├── Dockerfile          # imagen nginx:alpine
├── docker-compose.yml  # despliegue local
├── README.md           # documentación
├── LICENSE             # MIT
└── AGENTS.md           # este archivo
```

## Convenciones

- Idioma del contenido: español (la página es `lang="es"`).
- Estilo: CSS inline en `index.html`; mantenerlo autocontenido salvo decisión explícita de extraer a `styles.css`.
- Commits en español o inglés, formato corto imperativo (ej. `docs: agrega README profesional`).
- Nunca inventar funcionalidades, URLs de demo, emails o redes sociales: usar `TODO` si falta información.

## Reglas para modificar código

1. Trabajar siempre en rama `chore/*`, `docs/*`, `feat/*` o `fix/*`. Jamás directo a `main`.
2. Antes de editar: `git status` y `git branch --show-current`.
3. Cambios mínimos y reversibles; no borrar archivos sin aprobación.
4. No tocar `.env`, secretos ni credenciales (este repo no debe tener ninguno).
5. Actualizar `README.md` si cambia instalación, uso o arquitectura.

## Testing / Linting / Build

- Sin tests automatizados. Verificación mínima: build de Docker + `curl` HTTP 200 (ver README).
- Si se agrega CI, debe hacer: `docker build` + chequeo HTTP 200. No agregar frameworks de test sin aprobación.
- No ejecutar builds pesados si `/` tiene poco espacio (estaba al 87% el 2026-09-10).

## Deployment

Solo local vía Compose. Para producción: misma imagen tras reverse proxy con TLS. No hacer push de imágenes ni configurar registries sin aprobación.

## Variables de entorno

Ninguna. Si alguna vez se necesitan, documentarlas aquí sin valores y añadirlas a `.gitignore` (`.env` ya ignorado).

## Instrucciones específicas para IA (OpenCode = implementación, Codex = revisión)

- OpenCode: propone plan antes de implementar; implementa solo lo aprobado en la rama actual.
- Codex (reviewer): clasifica hallazgos en CRITICAL / HIGH / MEDIUM / LOW / SUGGESTION. Bloquea merge ante CRITICAL (secretos, Nginx sirviendo rutas sensibles, Dockerfile con `latest` sin pin, documentación falsa).
- Revisar siempre: `Dockerfile`, `docker-compose.yml`, `index.html`, `README.md`.
- Verificación final antes de PR: `git status`, `git diff --stat`, `git diff`.
