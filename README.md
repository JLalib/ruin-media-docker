# 📉 RUIN.MEDIA Docker - Herramienta Degradación Calidad Imágenes y Audio

[![GitHub](https://img.shields.io/badge/GitHub-Korosys%2FRUIN.MEDIA-blue?logo=github)](https://github.com/Korosys/RUIN.MEDIA)
[![Docker](https://img.shields.io/badge/Docker-korosys%2Fruin.media-blue?logo=docker)](https://hub.docker.com/r/korosys/ruin.media)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

## 📋 Descripción general

**RUIN.MEDIA** es una aplicación web satírica autohospedada en Docker que proporciona herramientas para degradar deliberadamente la calidad de imágenes y audio mediante dos niveles de reducción. Todo el procesamiento se realiza en el navegador del cliente (**client-side**) sin almacenar ningún dato en el servidor, garantizando privacidad total (*privacy by design*).

Es un proyecto de humor técnico (*ShitCode*) que juega con la idea de "empeorar" contenido multimedia de forma intencional, ideal para crear memes de baja calidad, probar compresión o simplemente disfrutar de la degradación satírica.

- **Procesamiento 100% client-side** (JavaScript en el navegador)
- **Cero datos en servidor** (stateless, sin almacenamiento temporal)
- **Backend PHP ultra-ligero** en imagen Alpine Docker
- **Dos niveles de degradación**: Normal + Extra Ruined
- **Interfaz drag-and-drop** moderna e intuitiva

## ✨ Características principales

- 🖼️ **Degradación de imágenes**: Compresión JPEG aumentada, downsampling de resolución, posterización de colores, reducción de saturación
- 🔊 **Degradación de audio**: Reducción de bitrate, compresión MP3 agresiva, inyección de ruido, pitch shift
- ⚙️ **Dos presets de degradación**: Normal (moderado) + Extra Ruined (extremo)
- 🎯 **Interfaz drag-and-drop** para subida fácil e intuitiva
- ⚡ **Preview en tiempo real** 100% client-side (JavaScript)
- 📥 **Descarga directa** de resultados (JPEG / MP3) sin watermark
- 🔒 **Privacy-first**: Cero almacenamiento en servidor, procesamiento totalmente local
- 🐳 **Docker Alpine ligero**: Imagen mínima, ~50-200 MB RAM, listo para producción
- 📜 **MIT Open Source**: Código abierto, comunitario, satírico y funcional

## 📋 Requisitos del sistema

- ✅ Docker Engine 20.10+
- ✅ Docker Compose v2+
- ✅ 50 MB - 200 MB RAM mínimo (PHP muy ligero)
- ✅ 100 MB espacio en disco (solo imagen Docker)
- ✅ Puerto 80 (o personalizado) para UI web
- ✅ Navegador moderno (Chrome, Firefox, Safari, Edge)
- 🔧 Opcional: Caddy o nginx (para HTTPS reverse proxy)

## 🐳 Instalación

### Opción 1: Docker Compose Simple (Recomendado)

```bash
mkdir -p ruin-media
cd ruin-media
docker run -d \
  --name ruin-media \
  -p 8080:80 \
  korosys/ruin.media:latest
# Acceso: http://localhost:8080
```

### Opción 2: Docker Compose con Archivo (Independiente)

```bash
cat > docker-compose.yml << 'EOF'
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

### Opción 3: Con Caddy Reverse Proxy (HTTPS Automático)

```bash
cat > docker-compose.yml << 'EOF'
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

cat > Caddyfile << 'EOF'
ruin.tudominio.com {
  reverse_proxy ruin-media:80
}
EOF

docker compose up -d
```

## ⚙️ Configuración

1. **Zona horaria**: Variable `TZ` (ej: `Europe/Madrid`, `America/Mexico_City`, `UTC`)
2. **Puerto host**: Cambiar `"8080:80"` por `"PUERTO_DESEADO:80"` en `docker-compose.yml`
3. **Dominio HTTPS**: Editar `Caddyfile` con tu dominio real (requiere DNS apuntando al servidor)
4. **Healthcheck**: Verifica disponibilidad HTTP cada 30s (configurable en compose)
5. **Recursos**: Límites opcionales con `deploy.resources.limits.memory` en compose

## 🚀 Primeros pasos

1. **Acceder a la Web UI**
   - Abre `http://localhost:8080` (o `https://ruin.tudominio.com` con Caddy)
   - Verás la interfaz: zona drag-and-drop, selector de nivel (Normal / Extra Ruined), preview en vivo

2. **Degradar una imagen**
   - Arrastra un archivo JPG/PNG a la interfaz
   - Selecciona nivel: **Normal** o **Extra Ruined**
   - El preview se actualiza instantáneamente (procesamiento client-side)
   - Click **"Download"** para descargar el JPEG degradado

3. **Degradar audio**
   - Arrastra un archivo MP3/WAV a la interfaz
   - Selecciona nivel de degradación
   - El procesamiento ocurre en el navegador
   - Descarga el MP3 con bitrate reducido

4. **Usar preset Extra Ruined**
   - Activa el toggle **"Extra Ruined"**
   - Imágenes: Compresión JPEG calidad 10-20%, posterización extrema
   - Audio: Bitrate ~32 kbps mono, ruido inyectado, pitch shift
   - Resultado: **Máxima degeneración satírica**

5. **Compartir resultado degradado**
   - Descarga el archivo `.jpg` o `.mp3` resultante
   - Comparte en Discord, Twitter, foros de memes, etc.
   - ¡Humor técnico total garantizado!

## 💡 Casos de uso

- 🎭 **Memes de baja calidad**: Crear *"cursed images"* intencionalmente degradadas
- 🧪 **Tests de compresión**: Ver cómo se ve contenido en distintos niveles de degradación
- 📼 **Simulación legacy**: Emular archivos viejos de baja calidad (estética 90s/2000s)
- 📊 **Tests de bandwidth**: Comparar streaming audio comprimido vs original
- 🎓 **Proyecto educativo satírico**: Entender compresión JPEG + encoding MP3 de forma visual
- 🎨 **Art project**: Degradación como statement artístico digital
- 😄 **Humor técnico**: Proyecto funcional que no se toma en serio a sí mismo

## 🔒 Acceso remoto seguro

La **Opción 3 (Caddy)** proporciona HTTPS automático con Let's Encrypt:

- Certificados TLS válidos y renovación automática
- Reverse proxy seguro hacia el contenedor interno
- Solo expone puertos 80/443 en el host
- Configuración en `Caddyfile`: `ruin.tudominio.com { reverse_proxy ruin-media:80 }`

**Requisitos**: Dominio válido + DNS A/AAAA apuntando a IP del servidor + puertos 80/443 abiertos.

## 🛠️ Gestión y mantenimiento

```bash
# Ver logs en tiempo real
docker logs -f ruin-media

# Reiniciar contenedor
docker compose restart ruin-media

# Actualizar imagen
docker pull korosys/ruin.media:latest
docker compose up -d

# Monitorear consumo de recursos
docker stats ruin-media
# Típico: ~10-30 MB RAM, CPU ~0%

# Health check manual
curl http://localhost:8080
# Debe retornar HTML 200 OK
```

## 📝 Licencia

**MIT License** - Proyecto open source satírico y funcional.

- Código fuente: [Korosys/RUIN.MEDIA](https://github.com/Korosys/RUIN.MEDIA)
- Docker Hub: [korosys/ruin.media](https://hub.docker.com/r/korosys/ruin.media)
- Issues & Discusiones: [GitHub Issues](https://github.com/Korosys/RUIN.MEDIA/issues)

---

> 📖 **Guía completa en el blog**: [Cómo instalar RUIN.MEDIA en Docker - Herramienta degradación calidad imágenes y audio autohospedada](https://genbyte.blogspot.com/2026/08/como-instalar-ruinmedia-en-docker.html)