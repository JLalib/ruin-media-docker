# 📉 RUIN.MEDIA Docker | Degradación satírica de imágenes y audio

[![GitHub](https://img.shields.io/badge/GitHub-Korosys%2FRUIN.MEDIA-181717?logo=github)](https://github.com/Korosys/RUIN.MEDIA)
[![Docker](https://img.shields.io/badge/Docker-korosys%2Fruin.media-2496ED?logo=docker)](https://hub.docker.com/r/korosys/ruin.media)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📋 Descripción general

**RUIN.MEDIA** es una aplicación web satírica autohospedada en Docker que proporciona herramientas para degradar deliberadamente la calidad de imágenes y audio mediante dos niveles de reducción. Todo el procesamiento se realiza en el navegador del cliente (**client-side**) sin almacenar ningún dato en el servidor. Es un proyecto de humor ("ShitCode") que juega con la idea de "empeorar" contenido multimedia de forma intencional, manteniendo una arquitectura **privacy-by-design** y **stateless**.

## ✨ Características principales

- 🖼️ **Degradación de imágenes**: compresión JPEG agresiva, downsampling de resolución, posterización de color, reducción de saturación
- 🔊 **Degradación de audio**: reducción de bitrate, codificación MP3 agresiva, inyección de ruido, pitch shift
- ⚙️ **Dos presets de degradación**: *Normal* (moderada) y *Extra Ruined* (extrema)
- 🖱️ **Interfaz drag-and-drop** intuitiva para subida de archivos
- ⚡ **Vista previa en tiempo real** 100% client-side (JavaScript)
- 📥 **Descarga directa** de resultados (JPEG / MP3) sin marca de agua
- 🔒 **Cero almacenamiento en servidor**: procesamiento stateless, privacy-first
- 🐳 **Imagen Docker Alpine ultra-ligera** (PHP 7.4+ backend mínimo)
- 📦 **Despliegue simple** con Docker Compose (un solo contenedor)
- 🎭 **Proyecto satírico open source** (licencia MIT) — funcional y divertido

## 📋 Requisitos del sistema

- Docker Engine 20.10+
- Docker Compose v2+
- 50–200 MB RAM (PHP extremadamente ligero)
- 100 MB espacio en disco (solo imagen Docker)
- Puerto 80 (o personalizado) para la UI web
- Navegador moderno: Chrome, Firefox, Safari, Edge
- *Opcional*: Caddy / nginx para reverse proxy con HTTPS

## 🐳 Instalación

### Opción 1: Docker Compose simple (recomendado)

```bash
mkdir -p ruin-media && cd ruin-media
docker run -d \
  --name ruin-media \
  -p 8080:80 \
  korosys/ruin.media:latest
# Acceso: http://localhost:8080
```

### Opción 2: Docker Compose con healthcheck (independiente)

```bash
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

### Opción 3: Con Caddy reverse proxy (HTTPS automático)

```bash
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

## ⚙️ Configuración

1. **Zona horaria**: variable `TZ` (ej. `Europe/Madrid`) en `docker-compose.yml`
2. **Puerto expuesto**: cambia `"8080:80"` por el puerto host deseado
3. **Dominio HTTPS**: edita `Caddyfile` con tu dominio real (`ruin.tudominio.com`)
4. **Healthcheck**: intervalo/timeout/retries ajustables en `docker-compose.yml`
5. **Recursos**: límites opcionales con `deploy.resources.limits` en compose v3.8+

## 🚀 Primeros pasos

1. **Acceder a la UI web**  
   Abre `http://localhost:8080` (o `https://ruin.tudominio.com` con Caddy). Verás la interfaz drag-and-drop y el selector de nivel de degradación.

2. **Degradar una imagen**  
   Arrastra un archivo JPG/PNG → elige *Normal* o *Extra Ruined* → la vista previa se actualiza al instante (client-side) → clic en **Download** para guardar el JPEG degradado.

3. **Degradar audio**  
   Arrastra un MP3/WAV → selecciona nivel → el procesamiento ocurre en el navegador → descarga el MP3 con bitrate reducido.

4. **Usar preset *Extra Ruined***  
   Activa el toggle *Extra Ruined* → compresión JPEG calidad 10–20% + audio ~32 kbps mono → máxima degeneración satírica.

5. **Compartir el resultado**  
   Descarga el `.jpg` o `.mp3` generado y compártelo en Discord, Twitter, foros de memes… humor técnico garantizado.

## 💡 Casos de uso

- 🎭 **Memes de baja calidad**: crear *cursed images* y audio intencionalmente terrible
- 🧪 **Tests de compresión**: visualizar cómo se ve/escucha contenido en distintos niveles de degradación
- 📼 **Simulación legacy**: emular archivos de los 90s / baja calidad intencional
- 📊 **Pruebas de ancho de banda**: comparar streaming de audio comprimido vs original
- 🎓 **Proyecto educativo satírico**: entender compresión JPEG y codificación MP3 de forma lúdica
- 🖼️ **Arte digital**: degradación como statement artístico / *glitch art*

## 🔒 Acceso remoto seguro

La opción 3 (Caddy) proporciona **HTTPS automático** con Let's Encrypt, certificados renovados automáticamente y reverse proxy hacia el contenedor interno. Solo necesitas un dominio apuntando a tu servidor y editar el `Caddyfile`.

## 🛠️ Gestión y mantenimiento

```bash
# Ver logs en tiempo real
docker logs -f ruin-media

# Reiniciar contenedor
docker compose restart ruin-media

# Actualizar imagen
docker pull korosys/ruin.media:latest && docker compose up -d

# Monitorizar consumo
docker stats ruin-media
# Típico: <50 MB RAM, CPU ~0%

# Health check manual
curl http://localhost:8080
# Debe retornar HTML 200 OK
```

## 📝 Licencia

MIT License — proyecto satírico open source. Ver [LICENSE](https://github.com/Korosys/RUIN.MEDIA/blob/main/LICENSE) en el repo oficial.

---

> 📖 **Artículo original**: [Cómo instalar RUIN.MEDIA en Docker - Herramienta degradación calidad imágenes y audio autohospedada](https://genbyte.blogspot.com/2026/08/como-instalar-ruinmedia-en-docker.html)  
> 🐙 **Repo oficial**: [Korosys/RUIN.MEDIA](https://github.com/Korosys/RUIN.MEDIA) | 🐳 **Docker Hub**: [korosys/ruin.media](https://hub.docker.com/r/korosys/ruin.media)