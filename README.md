# 📉 RUIN.MEDIA Docker - Herramienta Degradación Calidad Imágenes y Audio Autohospedada

[![GitHub](https://img.shields.io/badge/GitHub-Korosys%2FRUIN.MEDIA-181717?logo=github)](https://github.com/Korosys/RUIN.MEDIA)
[![Docker](https://img.shields.io/badge/Docker-korosys%2Fruin.media-2496ED?logo=docker)](https://hub.docker.com/r/korosys/ruin.media)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📋 Descripción general

**RUIN.MEDIA** es una aplicación web satírica autohospedada en Docker que proporciona herramientas para degradar deliberadamente la calidad de imágenes y audio mediante dos niveles de reducción. Todo el procesamiento se realiza en el navegador del cliente (**client-side**) sin almacenar ningún dato en el servidor, garantizando **privacy by design**.

Es un proyecto de humor técnico ("ShitCode") que juega con la idea de "empeorar" contenido multimedia de forma intencional, funcionando como una herramienta real para crear memes de baja calidad, test de compresión o simplemente experimentar con degradación digital.

- **Procesamiento 100% client-side** (JavaScript en el navegador)
- **Cero datos en servidor** (stateless, sin almacenamiento temporal)
- **Backend PHP ultra-ligero** sobre imagen Alpine Docker
- **Dos niveles de degradación**: Normal + Extra Ruined
- **Interfaz drag-and-drop** moderna e intuitiva

## ✨ Características principales

- 🖼️ **Degradación de imágenes**: Compresión JPEG aumentada, downsampling de resolución, posterización de colores, reducción de saturación
- 🔊 **Degradación de audio**: Reducción de bitrate, codificación MP3 agresiva, inyección de ruido, pitch shift
- ⚙️ **Dos presets de degradación**: Normal (moderado) + Extra Ruined (extremo)
- 🎯 **Interfaz drag-and-drop** para subida fácil e intuitiva
- ⚡ **Preview en tiempo real** (procesamiento client-side instantáneo)
- 📥 **Descarga directa** de resultados (JPEG/MP3) sin watermark
- 🔒 **Privacy-first**: Cero almacenamiento en servidor, procesamiento 100% en navegador
- 🐳 **Docker Alpine ligero**: Imagen mínima, ~50-200 MB RAM, listo para producción
- 📜 **MIT Open Source**: Proyecto satírico funcional y community-driven

## 📋 Requisitos del sistema

- ✅ Docker Engine 20.10+
- ✅ Docker Compose v2+
- ✅ 50 MB - 200 MB RAM mínimo (PHP muy ligero)
- ✅ 100 MB espacio en disco (solo imagen Docker)
- ✅ Puerto 80 (o personalizado) para Web UI
- ✅ Navegador moderno (Chrome, Firefox, Safari, Edge)
- 🔧 **Opcional**: Caddy o nginx (para HTTPS reverse proxy)

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
mkdir -p ruin-media
cd ruin-media
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
mkdir -p ruin-media
cd ruin-media
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

1. **Zona horaria**: Modifica `TZ=Europe/Madrid` en `docker-compose.yml` según tu ubicación
2. **Puerto local**: Cambia `"8080:80"` por `"PUERTO_DESEADO:80"` si el 8080 está ocupado
3. **Dominio HTTPS**: En `Caddyfile`, reemplaza `ruin.tudominio.com` por tu dominio real (requiere DNS apuntando al servidor)
4. **Healthcheck**: El contenedor incluye verificación automática cada 30s contra `http://localhost:80`
5. **Red**: Por defecto usa red bridge de Docker; para redes personalizadas añade `networks:` en compose

## 🚀 Primeros pasos

1. **Acceder a la Web UI**
   - Abre `http://localhost:8080` (o `https://ruin.tudominio.com` con Caddy)
   - Verás la interfaz: zona drag-and-drop, selector de nivel (Normal / Extra Ruined), preview en vivo

2. **Degradar una imagen**
   - Arrastra un archivo JPG/PNG a la zona de subida
   - Selecciona nivel: **Normal** o **Extra Ruined**
   - El preview se actualiza instantáneamente (procesamiento client-side)
   - Haz clic en **"Download"** para descargar el JPEG degradado

3. **Degradar audio**
   - Arrastra un archivo MP3/WAV a la interfaz
   - Selecciona nivel de degradación
   - El procesamiento ocurre en el navegador
   - Descarga el MP3 con bitrate reducido

4. **Usar preset Extra Ruined**
   - Activa el toggle **"Extra Ruined"**
   - Imágenes: Compresión JPEG calidad 10-20%, máxima posterización
   - Audio: Bitrate ~32 kbps mono, noise injection, pitch shift
   - Resultado: **Máxima degeneración satírica**

5. **Compartir resultado degradado**
   - Descarga el archivo `.jpg` o `.mp3` generado
   - Comparte en Discord, Twitter, foros de memes, etc.
   - ¡Humor técnico total garantizado!

## 💡 Casos de uso

- 🎭 **Memes de baja calidad**: Crear "cursed images" intencionalmente degradadas para humor en internet
- 🧪 **Tests de compresión**: Ver cómo se ve contenido en distintos niveles de degradación JPEG/MP3
- 📼 **Simulación legacy**: Emular archivos viejos de baja calidad (estética 90s/2000s)
- 📊 **Tests de bandwidth**: Comparar streaming de audio comprimido vs original
- 🎓 **Proyecto educativo satírico**: Entender compresión JPEG + encoding MP3 de forma visual
- 🎨 **Art project digital**: Degradación como statement artístico sobre calidad digital
- 🔒 **Privacy testing**: Verificar procesamiento 100% client-side sin fuga de datos

## 🔒 Acceso remoto seguro

Para exponer RUIN.MEDIA de forma segura en internet:

1. **Usa la Opción 3 (Caddy)**: HTTPS automático con Let's Encrypt, solo necesitas un dominio
2. **Autenticación básica** (opcional): Añade en `Caddyfile`:
   ```
   ruin.tudominio.com {
     basicauth {
       usuario hash_bcrypt
     }
     reverse_proxy ruin-media:80
   }
   ```
3. **Firewall**: Limita acceso al puerto 8080 solo a tu VPN/Tailscale si no usas Caddy
4. **Headers de seguridad**: Caddy añade automáticamente HSTS, CSP, etc.

## 🛠️ Gestión y mantenimiento

```bash
# Ver logs en tiempo real
docker logs -f ruin-media

# Reiniciar contenedor
docker compose restart ruin-media

# Actualizar a última versión
docker pull korosys/ruin.media:latest
docker compose up -d

# Monitorear consumo de recursos
docker stats ruin-media
# Típico: ~10-30 MB RAM, <1% CPU

# Health check manual
curl http://localhost:8080
# Debe retornar HTML 200 OK
```

## 📝 Licencia

Este proyecto está bajo licencia **MIT** - Ver [LICENSE](https://github.com/Korosys/RUIN.MEDIA/blob/main/LICENSE) para detalles.

Proyecto original: [Korosys/RUIN.MEDIA](https://github.com/Korosys/RUIN.MEDIA) | Imagen Docker: [korosys/ruin.media](https://hub.docker.com/r/korosys/ruin.media)

---

> 📖 **Guía completa en el blog**: [Cómo instalar RUIN.MEDIA en Docker - Herramienta degradación calidad imágenes y audio autohospedada](https://genbyte.blogspot.com/2026/08/como-instalar-ruinmedia-en-docker.html)