


vps-6108861-x.dattaweb.com

IP
149.50.152.162

Usuario:
root 

Puerto:
5430

Contraseña:
K5k40!C7%894BQC9cRMOODT



ssh -p 5430 root@149.50.152.162

cd /var/www/nest-app/backend


sudo apt update
sudo apt upgrade -y

Instalar Git:

sudo apt install git -y

Instalar Docker:

curl -fsSL https://get.docker.com | sh

Verificar:

docker --version

Instalar Docker Compose:

sudo apt install docker-compose-plugin -y

Verificar:

docker compose version
Paso 2. Crear carpeta de proyectos
sudo mkdir -p /var/www
cd /var/www/nest-app

sudo nano docker-compose.yaml


mysql password 

docker exec -it mysql mysql -u root -p
MYSQL_ROOT_PASSWORD=4BQC9cRMOODTK5k40!C7%89

sudo nano docker-compose.yaml

nslookup vpn.midominio.com
nslookup services.modotransformacion.click


Configurar Nginx
Paso 4. Crear Virtual Host

Supongamos que usarás:

vpn.midominio.com

Crear archivo:

sudo nano /etc/nginx/sites-available/services.modotransformacion.click
1. Reemplaza el contenido de /etc/nginx/sites-available/services.modotransformacion.click
server {
    listen 80;
    server_name services.modotransformacion.click;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name services.modotransformacion.click;

    ssl_certificate /etc/letsencrypt/live/services.modotransformacion.click/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/services.modotransformacion.click/privkey.pem;

    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://127.0.0.1:8080;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
2. Elimina el sitio por defecto
sudo rm /etc/nginx/sites-enabled/default
3. Habilita el sitio correcto
sudo ln -s /etc/nginx/sites-available/services.modotransformacion.click \
/etc/nginx/sites-enabled/services.modotransformacion.click
4. Comprueba la configuración
sudo nginx -t

Debe decir:

syntax is ok
test is successful
5. Recarga Nginx
sudo systemctl reload nginx
6. Prueba
curl https://services.modotransformacion.click

Deberías ver:

Hello World!

o la respuesta de tu API.

sudo certbot --nginx -d services.modotransformacion.click


curl services.modotransformacion.click