# Pasos para agregar SSL a un dominio en Nginx con Let's Encrypt

## 1. Instalar Certbot y el plugin de Nginx

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

## 2. Crear un bloque de servidor en Nginx

```bash
sudo nano /etc/nginx/sites-available/tu-dominio
```

Ejemplo de configuración básica:

```nginx
server {
    listen 80;
    server_name tu-dominio.com www.tu-dominio.com;

    location / {
        proxy_pass http://localhost:3000; # Cambia el puerto si es necesario
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Guardar y salir (`CTRL + X`, `Y`, `Enter`).

## 3. Habilitar el bloque de servidor

```bash
sudo ln -s /etc/nginx/sites-available/tu-dominio /etc/nginx/sites-enabled/
```

## 4. Probar la configuración de Nginx

```bash
sudo nginx -t
```

Si todo está bien, reiniciar Nginx:

```bash
sudo systemctl restart nginx
```

## 5. Obtener el certificado SSL

```bash
sudo certbot --nginx -d tu-dominio.com -d www.tu-dominio.com
```

Si ya existe un certificado y deseas reinstalarlo:

```bash
sudo certbot install --cert-name tu-dominio.com
```

## 6. Verificar la instalación del certificado

```bash
sudo certbot certificates
```

## 7. Configurar renovación automática

Let's Encrypt renueva automáticamente los certificados cada 90 días. Para probar la renovación manualmente:

```bash
sudo certbot renew --dry-run
```

Si todo funciona correctamente, la renovación automática está funcionando.

## 8. Forzar redirección a HTTPS (opcional)

Editar el archivo de configuración de Nginx (`/etc/nginx/sites-available/tu-dominio`) y agregar lo siguiente:

```nginx
server {
    listen 80;
    server_name tu-dominio.com www.tu-dominio.com;
    return 301 https://$host$request_uri;
}
```

Guardar, probar la configuración y reiniciar Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
```
