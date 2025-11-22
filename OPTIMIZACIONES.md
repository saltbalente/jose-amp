# Optimizaciones Realizadas y Recomendaciones

## ✅ Errores AMP Corregidos

### 1. Meta Description
- **Problema**: El documento no tenía una metadescripción
- **Solución**: Añadida meta description optimizada para SEO
```html
<meta name="description" content="Amarres de amor efectivos con el Maestro Evaristo...">
```

### 2. Canonical URL
- **Problema**: URL canónica relativa (.) en lugar de absoluta
- **Solución**: Cambiada a URL absoluta
```html
<link rel="canonical" href="https://ampevaristo.vercel.app/">
```

### 3. Scripts AMP
- **Problema**: Scripts no utilizados y falta de amp-video
- **Solución**: 
  - ✅ Añadido: `amp-video`
  - ✅ Mantenidos: `amp-lightbox`, `amp-form`, `amp-bind`
  - ❌ Eliminados: `amp-iframe`, `amp-position-observer`, `amp-animation`, `amp-social-share`, `amp-accordion`

### 4. Atributos Role y Tabindex
- **Problema**: Elementos con `on=` sin role/tabindex obligatorios
- **Solución**: Añadidos a todos los elementos interactivos:
  - Botones de play de video: `role="button" tabindex="0"`
  - Modales: `role="dialog" tabindex="-1"`
  - Contenido modal: `role="document" tabindex="0"`
  - Botones de cierre: `role="button" tabindex="0"`
  - Tarjetas de servicios: `role="button" tabindex="0"`

### 5. Error CSS
- **Problema**: `margin-top=33px` (sintaxis incorrecta)
- **Solución**: Corregido a `margin-top:33px`

## 📊 Optimizaciones de Caché Recomendadas

### Imágenes de GitHub Raw
Las imágenes alojadas en `raw.githubusercontent.com` tienen TTL de solo 5 minutos. Se recomienda:

#### Opción 1: Usar Vercel directamente (RECOMENDADO)
1. Crear carpeta `/public/images/` en el proyecto
2. Mover todas las imágenes allí
3. Actualizar URLs en el HTML

#### Opción 2: Usar un CDN
- Cloudinary (gratis hasta 25GB)
- ImageKit (gratis hasta 20GB)
- Cloudflare Images

### Videos
**Problema Crítico**: `video1.mp4` pesa 6.3MB
- Comprimir usando HandBrake o FFmpeg
- Objetivo: <2MB manteniendo calidad aceptable
- Comando FFmpeg sugerido:
```bash
ffmpeg -i video1.mp4 -c:v libx264 -crf 28 -preset slow -c:a aac -b:a 128k video1_optimized.mp4
```

### Imágenes de Blogger
Todas tienen cache de 1 día (bueno), pero se pueden optimizar:
- Convertir JPG a WebP (ahorro ~30%)
- Redimensionar al tamaño exacto necesario
- Comprimir con TinyPNG o Squoosh

## 🚀 Próximos Pasos

1. **Validar en AMP Validator**
   - Visita: https://validator.ampproject.org/
   - Introduce: https://ampevaristo.vercel.app/

2. **Probar en PageSpeed Insights**
   - https://pagespeed.web.dev/
   - Objetivo: >90 en móvil

3. **Optimizar Recursos**
   - Mover imágenes a `/public/images/`
   - Comprimir videos
   - Convertir imágenes a WebP

4. **Headers de Cache en Vercel**
   Actualizar `vercel.json`:
```json
{
  "headers": [
    {
      "source": "/images/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ]
}
```

## 📝 Comandos Útiles

### Validar AMP localmente
```bash
npx amphtml-validator index.html
```

### Comprimir imágenes
```bash
# Instalar herramienta
npm install -g imagemin-cli

# Comprimir
imagemin images/*.jpg --out-dir=images/optimized --plugin=mozjpeg
```

### Ver tamaño de archivos
```bash
du -h public/images/*
```
