# Tutorial: Apuntar un dominio a una IP mediante DNS en DreamHost

## 🎯 Objetivo

Configurar un dominio administrado en **DreamHost** para que apunte a una dirección IPv4 pública donde se encuentra alojado nuestro servidor.

### Ejemplo

Supongamos que tenemos:

```text
Dominio:
midominio.com

IP pública del servidor:
203.0.113.50
```

Queremos conseguir:

```text
https://midominio.com
        ↓
203.0.113.50
```

Y también:

```text
https://www.midominio.com
        ↓
203.0.113.50
```

---

# 1. Conceptos básicos

Para apuntar un dominio directamente a una dirección IPv4 se utiliza un registro:

```text
A
```

Un registro `A` relaciona un nombre de dominio con una dirección IPv4.

Por ejemplo:

```text
midominio.com → 203.0.113.50
```

DreamHost permite crear registros A personalizados desde la configuración DNS del dominio.

---

# 2. Antes de comenzar

Necesitamos conocer:

* El dominio.
* La IP pública del servidor.
* Acceso administrativo al panel de DreamHost.
* Saber si el servidor utiliza IPv4.
* Saber si actualmente existen registros DNS para el dominio.

Ejemplo:

```text
Dominio: empresa.com
IP: 203.0.113.50
```

> ⚠️ La IP debe ser la IP pública del servidor, no una IP privada como `192.168.x.x`, `10.x.x.x` o `172.16.x.x`.

---

# 3. Ingresar a DreamHost

Ingresar al panel administrativo de DreamHost.

Después:

```text
Manage Websites
        ↓
Buscar el dominio
        ↓
DNS Settings
```

DreamHost actualmente permite acceder a **DNS Settings** desde los tres puntos que aparecen junto al dominio en la sección **Manage Websites**.

---

# 4. Revisar la configuración actual

Antes de modificar cualquier cosa, revisar los registros existentes.

Podemos encontrar registros similares a:

```text
A       @       123.123.123.123
A       www     123.123.123.123
MX      @       mail.example.com
TXT     @       ...
CNAME   ...
```

Es importante revisar especialmente:

```text
A
CNAME
MX
TXT
```

porque algunos pueden estar relacionados con el sitio web, correo electrónico, verificaciones o servicios externos.

---

# 5. Configurar el dominio raíz

Para que:

```text
https://midominio.com
```

apunte a:

```text
203.0.113.50
```

debemos crear o modificar un registro:

```text
Tipo: A
Host: @
Apunta a: 203.0.113.50
```

En DreamHost, para el dominio raíz, el campo **Host** puede dejarse vacío. DreamHost interpreta el campo vacío como `@`.

### Configuración

```text
Type:
A

Host:
[vacío]

Points to:
203.0.113.50
```

Después seleccionar:

```text
Add Record
```

---

# 6. Configurar www

También debemos decidir qué ocurrirá con:

```text
www.midominio.com
```

Una opción sencilla es utilizar otro registro `A`:

```text
Tipo: A
Host: www
Apunta a: 203.0.113.50
```

Quedaría:

```text
midominio.com      → 203.0.113.50
www.midominio.com  → 203.0.113.50
```

DreamHost también permite utilizar un CNAME para `www`, pero un CNAME apunta a otro nombre de dominio, no directamente a una IP.

Por ejemplo:

```text
www → midominio.com
```

En ese caso:

```text
midominio.com → 203.0.113.50
www → midominio.com
```

---

# 7. Configuración recomendada

Para un servidor donde queremos controlar directamente la IP:

```text
┌───────────────┬──────┬────────────────┐
│ Host          │ Tipo │ Destino        │
├───────────────┼──────┼────────────────┤
│ @             │ A    │ 203.0.113.50   │
│ www           │ A    │ 203.0.113.50   │
└───────────────┴──────┴────────────────┘
```

También podemos utilizar:

```text
@    A       203.0.113.50
www  CNAME   midominio.com
```

---

# 8. ⚠️ Importante: DNS no configura el servidor

Apuntar el dominio a una IP **no configura automáticamente el servidor web**.

Por ejemplo, si tenemos:

```text
midominio.com
       ↓
203.0.113.50
       ↓
Servidor Linux
       ↓
Nginx
```

Nginx también debe estar configurado para responder al dominio.

Ejemplo:

```nginx
server {
    listen 80;
    server_name midominio.com www.midominio.com;

    root /var/www/html;

    index index.php index.html;
}
```

Por lo tanto existen dos configuraciones independientes:

```text
DNS
 │
 └── midominio.com → 203.0.113.50
                         │
                         ▼
                    Servidor Linux
                         │
                         ▼
                       Nginx
                         │
                         ▼
                  Aplicación web
```

---

# 9. Si utilizas HTTPS

Después de configurar el DNS, también debes configurar el certificado SSL.

La dirección final sería:

```text
https://midominio.com
```

En el servidor puedes utilizar, por ejemplo:

```text
Let's Encrypt
```

El certificado debe incluir:

```text
midominio.com
www.midominio.com
```

---

# 10. Verificar que el DNS funciona

Desde Windows podemos ejecutar:

```bash
nslookup midominio.com
```

Deberíamos obtener algo similar a:

```text
Name:    midominio.com
Address: 203.0.113.50
```

También podemos comprobar:

```bash
nslookup www.midominio.com
```

Esperamos:

```text
Name:    www.midominio.com
Address: 203.0.113.50
```

---

# 11. Verificar utilizando DNS

También podemos utilizar:

```bash
ping midominio.com
```

Aunque `ping` no siempre es una prueba definitiva de que el sitio web funciona, puede ayudarnos a comprobar que el dominio está resolviendo hacia la IP esperada.

Una prueba más apropiada es:

```bash
nslookup midominio.com
```

---

# 12. Verificar desde Linux

Desde un servidor Linux podemos utilizar:

```bash
dig midominio.com
```

O específicamente:

```bash
dig +short midominio.com
```

Resultado esperado:

```text
203.0.113.50
```

Para `www`:

```bash
dig +short www.midominio.com
```

Resultado:

```text
203.0.113.50
```

---

# 13. Verificar la configuración HTTP

Una vez que el DNS esté funcionando:

```bash
curl -I http://midominio.com
```

Podríamos obtener:

```text
HTTP/1.1 200 OK
Server: nginx
```

Esto indica que:

```text
Dominio
   ↓
DNS
   ↓
IP
   ↓
Servidor
   ↓
Nginx
   ↓
Aplicación
```

está funcionando.

---

# 14. Configuración completa de ejemplo

Supongamos:

```text
Dominio:
empresa.com

Servidor:
203.0.113.50
```

Configuramos:

```text
A
Host: [vacío]
Points to: 203.0.113.50
```

Y:

```text
A
Host: www
Points to: 203.0.113.50
```

Resultado:

```text
empresa.com
      ↓
203.0.113.50

www.empresa.com
      ↓
203.0.113.50
```

---

# 15. Si el servidor tiene varios dominios

Un mismo servidor puede alojar varios dominios.

Por ejemplo:

```text
empresa1.com
      ↓
203.0.113.50

empresa2.com
      ↓
203.0.113.50

empresa3.com
      ↓
203.0.113.50
```

Todos pueden apuntar a la misma IP.

Luego Nginx determina qué aplicación debe responder dependiendo del dominio:

```nginx
server {
    listen 80;
    server_name empresa1.com www.empresa1.com;

    root /var/www/empresa1;
}
```

Y:

```nginx
server {
    listen 80;
    server_name empresa2.com www.empresa2.com;

    root /var/www/empresa2;
}
```

---

# 16. ⚠️ No modificar los registros MX sin necesidad

Si el correo electrónico ya funciona con DreamHost, Google Workspace, Microsoft 365 u otro proveedor, debemos tener cuidado con los registros:

```text
MX
TXT
CNAME
DKIM
SPF
DMARC
```

Cambiar solamente el registro `A` permite mover el sitio web sin necesariamente mover el correo.

Esto es especialmente importante si el dominio ya tiene correo funcionando.

---

# 17. ¿Debo cambiar los Nameservers?

No necesariamente.

Si el DNS del dominio ya está administrado por DreamHost y solo queremos que el sitio web apunte a una IP determinada:

```text
NO necesitamos cambiar los nameservers.
```

Simplemente modificamos el registro `A`.

Los nameservers determinan **dónde se administra el DNS completo**, mientras que el registro A determina hacia qué IPv4 apunta un nombre concreto.

---

# 18. Diferencia entre Nameservers y registro A

### Nameservers

Ejemplo:

```text
ns1.dreamhost.com
ns2.dreamhost.com
ns3.dreamhost.com
```

Indican quién administra el DNS del dominio.

### Registro A

Ejemplo:

```text
empresa.com → 203.0.113.50
```

Indica a qué dirección IPv4 debe resolver el dominio.

Por eso:

```text
Nameserver
     ↓
¿Quién administra el DNS?

Registro A
     ↓
¿A qué IP apunta este dominio?
```

---

# 19. Propagación

Después de guardar el registro, los cambios DNS necesitan tiempo para propagarse.

DreamHost indica que los cambios de registros A pueden tardar varias horas en reflejarse globalmente, aunque el TTL de sus nameservers es de 5 minutos para determinadas actualizaciones.

Por eso, inmediatamente después de realizar el cambio, puede ocurrir que:

```text
Tu computador → IP nueva
Otro computador → IP anterior
```

durante un período de propagación.

---

# 20. Checklist final

Antes de terminar, comprobar:

* [ ] El dominio está administrado por DreamHost.
* [ ] El DNS está activo en DreamHost.
* [ ] Tenemos la IP pública correcta.
* [ ] Existe un registro `A` para el dominio raíz.
* [ ] Existe un registro para `www`.
* [ ] No eliminamos accidentalmente registros MX.
* [ ] El servidor permite tráfico por puerto `80`.
* [ ] El servidor permite tráfico por puerto `443`.
* [ ] Nginx/Apache está configurado con el dominio.
* [ ] El certificado SSL está configurado.
* [ ] `nslookup` devuelve la IP correcta.
* [ ] El sitio responde mediante HTTP/HTTPS.

---

# 📌 Resumen rápido

Si tienes:

```text
Dominio:
midominio.com

IP:
203.0.113.50
```

En DreamHost:

```text
Manage Websites
      ↓
DNS Settings
      ↓
Add Record
      ↓
A Record
```

Configurar:

```text
Host:      [vacío]
Type:      A
Points to: 203.0.113.50
```

Y para `www`:

```text
Host:      www
Type:      A
Points to: 203.0.113.50
```

Después comprobar:

```bash
nslookup midominio.com
```

Debe devolver:

```text
203.0.113.50
```

Y finalmente configurar el servidor web para que responda al dominio.

---

## Referencia oficial

Documentación oficial de DreamHost sobre registros DNS personalizados:

https://help.dreamhost.com/hc/es/articles/360035516812-Agregar-registros-de-DNS-personalizados
