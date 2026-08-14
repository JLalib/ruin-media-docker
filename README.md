# ��� RUIN.MEDIA Docker - Degradación Satírica de Imágenes y Audio

[![GitHub](https://img.shields.io/badge/GitHub-Korosys%2FRUIN.MEDIA-181717?logo=github)](https://github.com/Korosys/RUIN.MEDIA)
[![Docker](https://img.shields.io/badge/Docker-korosys%2Fruin.media-2496ED?logo=docker)](https://hub.docker.com/r/korosys/ruin.media)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## ��� Descripción general

**RUIN.MEDIA** es una aplicación web satírica autohospedada en Docker que proporciona herramientas para degradar deliberadamente la calidad de imágenes y audio mediante dos niveles de reducción. Todo el procesamiento se realiza en el navegador del cliente (**client-side**) sin almacenar ningún dato en el servidor, garantizando privacidad total (*privacy by design*).

Es un proyecto de humor técnico (*ShitCode*) que juega con la idea de "empeorar" contenido multimedia de forma intencional, ideal para crear memes de baja calidad, probar compresión o simplemente divertirse degradando archivos.

- **Imagen Docker**: `korosys/ruin.media:latest` (Alpine, ultra-ligera)
- **Puerto web**: 80 (HTTP)
- **Backend**: PHP 7.4+ minimalista
- **Procesamiento**: 100% JavaScript en el navegador
- **Almacenamiento servidor**: Cero (stateless)

## �� Características principales

- ������ **Degradación de imágenes**: Compresión JPEG agresiva, downsampling de resolución, posterización de color, reducción de saturación
- ��� **Degradación de audio**: Reducción de bitrate, codificación MP3 agresiva, inyección de ruido, pitch shift
- ������ **Dos presets de degradación**: *Normal* (moderado) y *Extra Ruined* (extremo)
- ������ **Interfaz drag-and-drop**: Subida intuitiva de archivos JPG/PNG/MP3/WAV
- ��� **Vista previa en tiempo real**: Comparación before/after instantánea (client-side)
- ��� **Descarga directa**: Exporta JPEG degradado o MP3 comprimido sin marca de agua
- ��� **Privacy-first**: Cero datos en servidor, sin archivos temporales, escalable horizontalmente
- ��� **Docker Alpine**: Imagen ultra-ligera (~50 MB), PHP minimalista, listo para producción
- ��� **MIT Open Source**: Proyecto satírico funcional, community-driven

## ��� Requisitos del sistema

- �� Docker Engine 20.10+
- �� Docker Compose v2+ (plugin `docker compose`)
- �� 50–200 MB RAM (PHP extremadamente ligero)
- �� 100 MB espacio en disco (solo imagen Docker)
- �� Puerto 80 libre (o personalizado via `ports:`)
- �� Navegador moderno: Chrome, Firefox, Safari, Edge (JS ES6+)
- ������ **Opcional**: Caddy / nginx para reverse proxy HTTPS

## ��� Instalación

### Opción 1: Docker Compose simple (recomendado)

```bash
mkdir -p ruin-media && cd ruin-media
cat > docker-compose.yml <<'EOF'
version: '3.8'

services:
  ruin-media:
    image: korosys/ruin.media:latest
    container_name: ruin-media
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      - TZ=Europe/Madrid
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:80"]
      interval: 30s
      timeout: 5s
      retries: 3
EOF

docker compose up -d
```

**Acceso**: `http://localhost:8080`

---

### Opción 2: Docker run rápido (una línea)

```bash
docker run -d \
  --name ruin-media \
  -p 8080:80 \
  -e TZ=Europe/Madrid \
  --restart unless-stopped \
  korosys/ruin.media:latest
```

---

### Opción 3: Con Caddy reverse proxy (HTTPS automático)

```bash
mkdir -p ruin-media && cd ruin-media

cat > docker-compose.yml <<'EOF'
version: '3.8'

services:
  ruin-media:
    image: korosys/ruin.media:latest
    container_name: ruin-media
    restart: unless-stopped
    environment:
      - TZ=Europe/Madrid

  caddy:
    image: caddy:latest
    container_name: ruin-media-caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - ruin-media

volumes:
  caddy_data:
  caddy_config:
EOF

cat > Caddyfile <<'EOF'
ruin.tudominio.com {
  reverse_proxy ruin-media:80
}
EOF

docker compose up -d
```

**Acceso**: `https://ruin.tudominio.com` (certificado TLS automático via Let's Encrypt)

## ������ Configuración

1. **Zona horaria**: Variable `TZ` (ej. `Europe/Madrid`, `America/Mexico_City`, `UTC`)
2. **Puerto host**: Cambia `"8080:80"` por `"80:80"` o `"3000:80"` según necesidad
3. **Healthcheck**: Verifica disponibilidad HTTP cada 30s (curl interno)
4. **Reverse proxy**: Si usas Caddy/Traefik/Nginx, elimina `ports:` en `ruin-media` y usa `expose: ["80"]`
5. **Sin volúmenes persistentes**: La app es stateless; no requiere bind mounts

## ��� Primeros pasos

1. **Accede a la Web UI**  
   Abre `http://localhost:8080` (o tu dominio HTTPS). Verás la interfaz drag-and-drop y selector de nivel: *Normal* / *Extra Ruined*.

2. **Degrada una imagen**  
   Arrastra un archivo **JPG/PNG** → selecciona nivel → vista previa instantánea → clic **"Download"** para obtener el JPEG degradado.

3. **Degrada audio**  
   Arrastra **MP3/WAV** → elige preset → procesamiento client-side → descarga **MP3** con bitrate reducido.

4. **Prueba *Extra Ruined***  
   Activa el toggle *Extra Ruined*: JPEG calidad 10–20%, audio ~32 kbps mono, máxima degeneración satírica.

5. **Comparte el resultado**  
   Descarga el `.jpg` o `.mp3` y compártelo en Discord, Twitter, foros de memes… humor técnico garantizado.

## ��� Casos de uso

- ��� **Memes "cursed"**: Crea imágenes/audio intencionalmente terribles para humor internético
- ��� **Tests de compresión**: Visualiza cómo se ve contenido en distintos niveles de degradación
- ��� **Simulación legacy**: Emula archivos de los 90s (JPEG artefactos, audio 32 kbps)
- ��� **Bandwidth testing**: Compara streaming de audio comprimido vs original
- ��� **Proyecto educativo satírico**: Entiende compresión JPEG y encoding MP3 de forma divertida
- ������ **Art statement**: Degradación como expresión artística digital intencional

## ��� Acceso remoto seguro

> **Recomendado**: Usa **Caddy** (Opción 3) para HTTPS automático con Let's Encrypt.  
> Alternativas: Traefik, Nginx Proxy Manager, Cloudflare Tunnel.  
> **Nunca** expongas el puerto 8080 directamente a Internet sin TLS.

## ������ Gestión y mantenimiento

```bash
# Ver logs en tiempo real
docker logs -f ruin-media

# Reiniciar contenedor
docker compose restart ruin-media

# Actualizar imagen
docker pull korosys/ruin.media:latest
docker compose up -d

# Monitor de recursos
docker stats ruin-media
# Típico: ~10-30 MB RAM, ~0.1% CPU

# Health check manual
curl -f http://localhost:8080  # Debe retornar HTML 200 OK
```

## ��� Licencia

**MIT License** — Proyecto open source satírico funcional.  
Código fuente: [Korosys/RUIN.MEDIA](https://github.com/Korosys/RUIN.MEDIA)  
Imagen Docker: [korosys/ruin.media](https://hub.docker.com/r/korosys/ruin.media)

---

> ��� **Artículo original**: [Cómo instalar RUIN.MEDIA en Docker - Genbyte](https://genbyte.blogspot.com/2026/08/como-instalar-ruinmedia-en-docker.html)