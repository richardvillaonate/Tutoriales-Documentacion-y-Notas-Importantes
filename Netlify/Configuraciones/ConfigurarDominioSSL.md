# Despliegue de Angular 16 en Netlify con Dominio de DreamHost mediante CNAME y SSL

## Objetivo

Este documento describe el proceso completo para desplegar una aplicación Angular en Netlify, conectar un dominio adquirido en DreamHost utilizando la opción de configuración por CNAME y habilitar SSL  mediante Let's Encrypt.

---

# 1. Preparación del Proyecto Angular

## Verificar el outputPath

Abrir:

```json
angular.json
```

Verificar que exista:

```json
"outputPath": "dist/nombredelproyecto"
```

Esta será la carpeta publicada por Netlify.

---

## Configurar soporte para rutas Angular (SPA)

Crear:

```text
src/_redirects
```

Contenido:

```text
/* /index.html 200
```

Verificar que esté incluido en:

```json
"assets": [
  "src/favicon.ico",
  "src/assets",
  "src/_redirects"
]
```

---

# 2. Crear el archivo netlify.toml

En la raíz del proyecto crear:

```text
netlify.toml
```

Contenido:

```toml
[build]
command = "npm run build"
publish = "dist/nombredelproyecto"

[[redirects]]
from = "/*"
to = "/index.html"
status = 200
```

---

# 3. Subir el Proyecto a GitHub

```bash
git add .
git commit -m "Configuracion Netlify"
git push origin main
```

---

# 4. Crear el Sitio en Netlify

1. Ingresar a Netlify.
2. Seleccionar **Add New Site**.
3. Seleccionar **Import an Existing Project**.
4. Conectar GitHub.
5. Seleccionar el repositorio.

---

# 5. Configuración de Despliegue

## IMPORTANTE

Durante la configuración inicial del proyecto en Netlify se debe especificar el comando de compilación.

### Build Command

```bash
npm run build
```

### Publish Directory

```text
dist/nombredelproyecto
```

### Base Directory

```text
(dejar vacío)
```

---

## Resumen

| Parámetro         | Valor                   |
| ----------------- | ----------------------- |
| Build Command     | npm run build           |
| Publish Directory | dist/nombredelproyecto |
| Base Directory    | Vacío                   |

---

# 6. Ejecutar el Primer Deploy

Netlify ejecutará:

```bash
npm install
npm run build
```

Y generará:

```text
dist/nombredelproyecto
```

Al finalizar obtendrás una URL similar a:

```text
https://nombre-sitio.netlify.app
```

Verifica que el sitio funcione correctamente.

---

# 7. Agregar el Dominio Personalizado

Ingresar a:

```text
Site Configuration
→ Domains
→ Add Domain
```

Agregar:

```text
midominio.com
```

Agregar también:

```text
www.midominio.com
```

---

# 8. Configurar el Dominio mediante la Opción CNAME Setup

Una vez agregado el dominio, Netlify mostrará varias opciones de configuración DNS.

Seleccionar:

```text
Use a domain I already own
```

Luego seleccionar:

```text
Use CNAME Setup
```

Netlify mostrará un registro similar a:

```text
www.midominio.com
CNAME
nombre-sitio.netlify.app
```

Y proporcionará las instrucciones para el dominio principal.

Esta opción permite mantener la administración DNS desde DreamHost sin delegar Nameservers a Netlify.

---

# 9. Configurar DNS en DreamHost

Ingresar a:

```text
Domains
→ Manage Domains
→ DNS
```

---

## Configuración del Subdominio WWW

Crear un registro:

```text
Tipo: CNAME
Host: www
Valor: nombre-sitio.netlify.app
```

Ejemplo:

```text
Tipo: CNAME
Host: www
Valor: modo-transformacion.netlify.app
```

---

## Configuración del Dominio Principal

Netlify mostrará dos registros A para el dominio raíz.

Crear:

```text
Tipo: A
Host: @
Valor: 75.2.60.5
```

Crear:

```text
Tipo: A
Host: @
Valor: 99.83.190.102
```

---

# 10. Esperar la Propagación DNS

Tiempo estimado:

```text
15 minutos a 24 horas
```

Normalmente:

```text
30 minutos a 2 horas
```

---

# 11. Verificar el Dominio en Netlify

Una vez propagados los registros DNS, Netlify mostrará:

```text
Domain Verified
```

---

# 12. Activar SSL Gratuito

Ingresar a:

```text
Site Configuration
→ Domains
→ HTTPS
```

Seleccionar:

```text
Verify DNS Configuration
```

Netlify emitirá automáticamente un certificado SSL de Let's Encrypt.

---

# 13. Forzar HTTPS

Activar:

```text
Force HTTPS
```

Con esto:

```text
http://midominio.com
```

se redireccionará automáticamente a:

```text
https://midominio.com
```

---

# 14. Configurar Dominio Principal

Definir:

```text
midominio.com
```

como dominio principal.

Y dejar:

```text
www.midominio.com
```

como alias.

Netlify realizará automáticamente la redirección correspondiente.

---

# 15. Validación Final

Verificar:

```text
https://midominio.com
```

```text
https://www.midominio.com
```

```text
https://midominio.com/login
```

```text
https://midominio.com/dashboard
```

```text
https://midominio.com/perfil
```

Recargar las páginas para confirmar que Angular maneja correctamente las rutas.

---

# Problemas Frecuentes

## Error

```text
No build steps found
```

Solución:

```bash
npm run build
```

debe estar configurado en **Build Command**.

---

## Error

```text
Deploy directory does not exist
```

Verificar:

```text
dist/nombredelproyecto
```

como **Publish Directory**.

---

## Error

```text
Page Not Found al refrescar
```

Verificar:

```text
src/_redirects
```

Contenido:

```text
/* /index.html 200
```

---

# Resultado Final

✅ Aplicación Angular desplegada en Netlify
✅ Integración continua con GitHub
✅ Dominio de DreamHost conectado mediante CNAME Setup
✅ DNS administrado desde DreamHost
✅ SSL gratuito Let's Encrypt
✅ HTTPS forzado
✅ Compatibilidad total con Angular SPA
