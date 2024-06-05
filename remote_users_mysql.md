## Configuración de MySQL para permitir conexiones remotas en Linux

### Paso 1: Configurar MySQL para aceptar conexiones remotas

1. **Editar el archivo de configuración de MySQL**:
   - Abre el archivo `mysqld.cnf`:
     ```bash
     sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
     ```
   - Modifica la línea `bind-address` para permitir conexiones desde todas las direcciones:
     ```ini
     bind-address = 0.0.0.0
     ```
   - Guarda y cierra el archivo.

2. **Reiniciar el servicio MySQL**:
   ```bash
   sudo systemctl restart mysql

### Paso 2: Crear un usuario con permisos para acceder remotamente

1. **Abrir la línea de comandos de MySQL**:
   ```bash
   sudo mysql -u root -p
   ```

2. **Crear un usuario con permisos para acceder desde cualquier host**:
   ```sql
   CREATE USER 'usuario_remoto'@'%' IDENTIFIED BY 'tu_contraseña';
   GRANT ALL PRIVILEGES ON *.* TO 'usuario_remoto'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```

### Paso 3: Configurar el firewall para permitir conexiones al puerto MySQL

1. **Abrir el puerto en el firewall**:
   - Usando `ufw`:
     ```bash
     sudo ufw allow 3306/tcp
     sudo ufw reload
     ```
   - Usando `firewalld`:
     ```bash
     sudo firewall-cmd --permanent --add-service=mysql
     sudo firewall-cmd --reload
     ```

### Paso 4: Verificar la conexión remota

1. **Intenta conectarte desde una máquina remota**:
   ```bash
   mysql -u usuario_remoto -p -h direccion_ip_del_servidor
   ```
   - Reemplaza `direccion_ip_del_servidor` con la IP de tu servidor MySQL y `usuario_remoto` con el nombre de usuario creado.

Si sigues estos pasos correctamente, podrás conectarte a tu servidor MySQL de forma remota.
```

Puedes copiar este contenido y guardarlo en tu repositorio en formato Markdown (.md). Si necesitas más ayuda, no dudes en preguntar.
