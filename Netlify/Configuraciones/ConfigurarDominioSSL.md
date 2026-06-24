# Guía Completa: Desplegar un Proyecto Angular en Netlify con Dominio de DreamHost y SSL Gratuito

## Objetivo

Al finalizar este tutorial tendrás:

* Aplicación Angular desplegada en Netlify.
* Dominio personalizado administrado desde DreamHost.
* Certificado SSL gratuito configurado automáticamente.
* Redirección HTTPS habilitada.
* Configuración para SPA (Single Page Application) Angular.

---

# 1. Construir el Proyecto Angular

Ubícate en la raíz del proyecto:

```bash
cd mi-proyecto-angular
```

Genera la versión de producción:

```bash
ng build --configuration production
```

Angular generará los archivos compilados dentro de:

```bash
dist/
```

Ejemplo:

```bash
dist/mi-aplicacion
```

---

# 2. Crear una Cuenta en Netlify

Ingresa a:

https://www.netlify.com

Puedes registrarte utilizando:

* GitHub
* Correo electrónico

---

# 3. Desplegar Manualmente en Netlify

## Método rápido

PARA TENER EN CUENTA

EN LA CONFIGURACION DE DESPLIEGUE 
DEBEMOS AGREGAR EL BULD COMMAND
npm run build

Y EL DIRECTORIO DE COMPILACION

dist/mi-aplicacion

Después de ejecutar el build:

```bash
ng build --configuration production
```

Ingresa al panel de Netlify:

```text
Sites → Add New Site → Deploy Manually
```

Arrastra la carpeta:

```bash
dist/mi-aplicacion
```

Netlify publicará el sitio y te entregará una URL temporal:

```text
https://nombre-aleatorio.netlify.app
```

Ejemplo:

```text
https://amazing-bird-12345.netlify.app
```

---

# 4. Configurar Angular para Rutas SPA

Angular utiliza rutas internas.

Si no se configura correctamente, al actualizar una página aparecerá:

```text
404 Not Found
```

## Crear archivo _redirects

Dentro de:

```bash
src/
```

Crear:

```bash
_redirects
```

Contenido:

```text
/* /index.html 200
```

---

## Modificar angular.json

Buscar:

```json
"assets": [
  "src/favicon.ico",
  "src/assets"
]
```

Agregar:

```json
"assets": [
  "src/favicon.ico",
  "src/assets",
  "src/_redirects"
]
```

Volver a compilar:

```bash
ng build --configuration production
```

---

# 5. Agregar Dominio Personalizado

Supongamos que el dominio es:

```text
midominio.com
```

Y está registrado en DreamHost.

---

## En Netlify

Entrar a:

```text
Site Settings
```

Luego:

```text
Domain Management
```

Seleccionar:

```text
Add Custom Domain
```

Agregar:

```text
midominio.com
```

Y luego:

```text
www.midominio.com
```

Netlify mostrará los DNS necesarios.

---

# 6. Configuración DNS en DreamHost

Ingresar a:

```text
DreamHost Panel
```

Luego:

```text
Domains
```

Seleccionar:

```text
Manage Domains
```

Abrir:

```text
DNS Settings
```

---

## Configuración para dominio raíz

Crear registros A.

### Registro 1

```text
Type: A
Host:
@
Value:
75.2.60.5
```

### Registro 2

```text
Type: A
Host:
@
Value:
99.83.190.102
```

---

## Configuración para WWW

Crear un CNAME:

```text
Type:
CNAME

Host:
www

Value:
tu-sitio.netlify.app
```

Ejemplo:

```text
www → amazing-bird-12345.netlify.app
```

---

# 7. Verificar DNS

Puede tardar:

```text
5 minutos a 24 horas
```

Verificar propagación:

https://dnschecker.org

Buscar:

```text
A
```

y

```text
CNAME
```

---

# 8. Activar SSL Gratuito

Una vez propagado el dominio:

Entrar a:

```text
Netlify
```

Luego:

```text
Domain Settings
```

Seleccionar:

```text
HTTPS
```

Después:

```text
Verify DNS Configuration
```

Netlify emitirá automáticamente un certificado:

```text
Let's Encrypt
```

Sin costo.

---

# 9. Forzar HTTPS

Ir a:

```text
Site Settings
```

Luego:

```text
HTTPS
```

Activar:

```text
Force HTTPS
```

Con esto:

```text
http://midominio.com
```

redireccionará automáticamente a:

```text
https://midominio.com
```

---

# 10. Configurar Redirección WWW → Dominio Principal

Recomendado para SEO.

Supongamos que el dominio principal será:

```text
https://midominio.com
```

Crear archivo:

```bash
src/_redirects
```

Contenido:

```text
https://www.midominio.com/* https://midominio.com/:splat 301!
/* /index.html 200
```

Volver a compilar:

```bash
ng build --configuration production
```

Subir nuevamente a Netlify.

---

# 11. Despliegue Automático desde GitHub

Recomendado para producción.

## Crear repositorio

```bash
git init
git add .
git commit -m "Primer despliegue"
git branch -M main
git remote add origin URL_REPOSITORIO
git push -u origin main
```

---

## Conectar GitHub a Netlify

En Netlify:

```text
Add New Site
```

Seleccionar:

```text
Import Existing Project
```

Elegir:

```text
GitHub
```

Seleccionar repositorio.

---

## Configuración Build

### Build Command

```bash
ng build --configuration production
```

### Publish Directory

```bash
dist/mi-aplicacion/browser
```

Angular 17+ suele generar:

```bash
dist/proyecto/browser
```

Verifica la carpeta exacta después del build.

---

# 12. Variables de Entorno (Opcional)

En Netlify:

```text
Site Settings
```

→

```text
Environment Variables
```

Agregar:

```text
API_URL=https://api.midominio.com
```

---

# 13. Validaciones Finales

Verificar:

### Dominio

```text
https://midominio.com
```

### WWW

```text
https://www.midominio.com
```

### SSL

Candado verde activo.

### Rutas Angular

Probar:

```text
https://midominio.com/login
```

```text
https://midominio.com/dashboard
```

```text
https://midominio.com/perfil
```

Recargar la página y confirmar que no aparezca error 404.

---

# Arquitectura Final

```text
Usuario
   │
   ▼
https://midominio.com
   │
   ▼
Netlify CDN
   │
   ▼
Angular Build
   │
   ▼
API Backend
```

Resultado:

✅ Hosting gratuito de Angular
✅ CDN global de Netlify
✅ SSL gratuito Let's Encrypt
✅ Dominio de DreamHost funcionando
✅ Despliegue automático desde GitHub
✅ Soporte completo para rutas Angular SPA
