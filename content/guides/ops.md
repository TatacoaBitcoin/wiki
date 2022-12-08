---
title: 'OPS'
metaTitle: 'Guías - OPS'
metaDescription: 'How-to guides'
---

# Verificación funcionamiento de servicios

Verificar uno a uno que los servicios en `tatacoa/Infraestructura/infraestructura.ods:Servicios` están corriendo sin problemas.

## Bad Gateway Error

Este error es devuelto por el servidor nginx. Posiblemente se debe a que el proceso al cual
redirecciona nginx no está respondiendo.

### Procedimiento

Si conoce la unidad del proceso al cual redirecciona nginx, verifique su estado con `systemctl status lnbitsmain.service`. Si el estado no es activo, inicie nuevamente el proceso con `systemctl status lnbitsmain.service` y habilítelo para que inicie automáticamente con `systemctl enable lnbitsmain.service`.

Si no conoce la unidad del proceso, haga lo siguiente:

1. Look on the config file of the service for the proxy pass destination, like `proxy_pass http://127.0.0.1:8004`.
2. Check there is something listening at that destination with something like `netstat -atlp`.
3. If nothing is listening there, go to the data file of the service and identify the type of service.
4. If the service is npm based, check with `pm2 ls` if it is running. If not, start it again with `pm2 start index.js`.

# Detener servicio prestashop

1. Deshabilitar sitio nginx con `rm /etc/nginx/sites-enabled/prestashop.tatacoabitcoin.com.conf`. Al quitar el enlace, se le dice a nginx que no exponga este sitio. Se puede conservar el archivo de configuración del dominio para futuras referencias.
2. Reiniciar nginx con `nginx -t && systemctl start nginx`.
3. Verifique la información de la base de datos en el archivo de configuración. Si no puede ver
   esta información. Liste e identifique la base de datos con `mysql -e "show databases;"`.
4. Borrar base de datos con `mysql -e "drop database prestashop;"`.
5. Borrar archivos del sitio con `rm -r /var/www/html/prestashop.tatacoabitcoin.com/`.
6. Eliminar entrada en el dominio.

   nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size: 64

   vi /etc/nginx/nginx.conf

   http {
   ...
   server_names_hash_bucket_size 128;
