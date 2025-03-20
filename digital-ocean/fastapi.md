# Cómo Ejecutar una Aplicación FastAPI con `systemd` (Guía Genérica)

Esta guía te explica cómo configurar y ejecutar una aplicación FastAPI como un servicio en Linux usando `systemd`.

---

## **Pasos para Configurar FastAPI con `systemd`**

### 1. **Crear un Archivo de Servicio**

1. Crea un archivo de servicio en el directorio `/etc/systemd/system/`. Por ejemplo:

   ```bash
   sudo nano /etc/systemd/system/mi_aplicacion.service
   ```

2. Agrega el siguiente contenido al archivo (ajusta las rutas y nombres según tu proyecto):

   ```ini
   [Unit]
   Description=Mi Aplicación FastAPI
   After=network.target

   [Service]
   User=tu_usuario
   WorkingDirectory=/ruta/a/tu/proyecto
   ExecStart=/ruta/a/tu/entorno/bin/uvicorn archivo_principal:app --host 0.0.0.0 --port 8000
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

   - **User**: El usuario bajo el cual se ejecutará el servicio (por ejemplo, `tu_usuario`).
   - **WorkingDirectory**: La ruta al directorio de tu proyecto.
   - **ExecStart**: El comando para iniciar tu aplicación FastAPI (usa la ruta completa a `uvicorn` dentro de tu entorno virtual).
   - **Restart=always**: Reinicia automáticamente el servicio si se detiene.

3. Guarda y cierra el archivo (`Ctrl + O`, luego `Ctrl + X` en `nano`).

---

### 2. **Recargar `systemd` y Habilitar el Servicio**

1. Recarga la configuración de `systemd` para que reconozca el nuevo servicio:

   ```bash
   sudo systemctl daemon-reload
   ```

2. Habilita el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable mi_aplicacion.service
   ```

3. Inicia el servicio:
   ```bash
   sudo systemctl start mi_aplicacion.service
   ```

---

### 3. **Verificar el Estado del Servicio**

Para verificar que el servicio está corriendo correctamente, usa el siguiente comando:

```bash
sudo systemctl status mi_aplicacion.service
```

Esto te mostrará información sobre el estado del servicio, incluyendo si está activo, inactivo o si hay algún error.

---

### 4. **Comandos Útiles para Gestionar el Servicio**

1. **Ver el estado del servicio**:

   ```bash
   sudo systemctl status mi_aplicacion.service
   ```

   Este comando te muestra si el servicio está activo, los logs recientes y cualquier error.

2. **Ver los logs del servicio**:

   ```bash
   sudo journalctl -u mi_aplicacion.service
   ```

   Este comando te permite ver los registros (logs) del servicio, lo que es útil para depurar problemas.

---

### **Otros Comandos Útiles**

- **Reiniciar el servicio**:

  ```bash
  sudo systemctl restart mi_aplicacion.service
  ```

- **Detener el servicio**:

  ```bash
  sudo systemctl stop mi_aplicacion.service
  ```

- **Habilitar el inicio automático**:

  ```bash
  sudo systemctl enable mi_aplicacion.service
  ```

- **Deshabilitar el inicio automático**:
  ```bash
  sudo systemctl disable mi_aplicacion.service
  ```

---

### **Comandos Clave**:

1. **Ver estado del servicio**:

   ```bash
   sudo systemctl status mi_aplicacion.service
   ```

2. **Ver logs del servicio**:
   ```bash
   sudo journalctl -u mi_aplicacion.service
   ```

### **Estructura del Archivo de Servicio**:

```ini
[Unit]
Description=Mi Aplicación FastAPI
After=network.target

[Service]
User=tu_usuario
WorkingDirectory=/ruta/a/tu/proyecto
ExecStart=/ruta/a/tu/entorno/bin/uvicorn archivo_principal:app --host 0.0.0.0 --port 8000
Restart=always

[Install]
WantedBy=multi-user.target
```

Fin :v
