### Configuración de Systemd Service para Ejecutar un Script de Python Manualmente

#### 1. Crear el Script `.sh`

Primero, crea un script `.sh` que ejecutará tu script de Python. Por ejemplo:

```bash
#!/bin/bash
python3 /ruta/completa/a/tu_script.py
```

Guarda este archivo como `start_bots.sh` y dale permisos de ejecución:

```bash
chmod +x /ruta/completa/a/start_bots.sh
```

#### 2. Crear el Archivo de Servicio `Systemd`

Ahora crea un archivo de servicio `systemd` que haga referencia a tu script `.sh`.

```bash
sudo nano /etc/systemd/system/bots.service
```

Añade la siguiente configuración al archivo:

```ini
[Unit]
Description=Script de Telegram
After=network.target

[Service]
ExecStart=/ruta/completa/a/start_bots.sh
WorkingDirectory=/ruta/completa/a/
Restart=always
User=tu_usuario
ExecStop=/bin/systemctl stop bots.service

StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=bots

[Install]
WantedBy=multi-user.target
```

Asegúrate de reemplazar `/ruta/completa/a/start_bots.sh` con la ruta completa a tu script `.sh` y `tu_usuario` con el usuario adecuado.

#### 3. Recargar los Servicios de `systemd`

Recarga los servicios de `systemd` para que detecte el nuevo archivo de servicio:

```bash
sudo systemctl daemon-reload
```

#### 4. Iniciar y Detener el Servicio Manualmente

Ahora puedes iniciar y detener el servicio manualmente cuando lo desees:

Para iniciar el servicio:

```bash
sudo systemctl start bots.service
```

Para detener el servicio:

```bash
sudo systemctl stop bots.service
```

Para verificar el estado del servicio:

```bash
sudo systemctl status bots.service
```

#### 5. Ver los Logs

Los logs se enviarán al sistema de registro (syslog) del sistema. Puedes ver los logs del servicio usando el comando `journalctl`:

```bash
journalctl -u bots.service
```

Con esta configuración, tendrás la flexibilidad de iniciar tu script de Python cuando lo desees, y podrás revisar los logs del servicio para obtener información sobre su ejecución y cualquier error que pueda ocurrir.