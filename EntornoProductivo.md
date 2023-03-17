## Pasos para virtualizar entorno productivo en Laravel

1. Abrir el archivo **000-default.conf** ubicado en la ruta **/etc/apache2/sites-available/000-default.conf** y pegar el siguiente código, reemplazando los campos correspondientes con la información de tu proyecto:

```

<VirtualHost \*:80>

ServerName {{ TUDOMINIO }}.com
ServerAlias www.{{ TUDOMINIO }}.com
ServerAdmin admin@{{ TUDOMINIO }}.com

DocumentRoot /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO}}/public

<Directory /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO}}/public>
Options -Indexes +FollowSymLinks
AllowOverride All
</Directory>

ErrorLog ${APACHE_LOG_DIR}/{{NOMBRE DEL PROYECTO}}\_error.log
CustomLog ${APACHE_LOG_DIR}/{{NOMBRE DEL PROYECTO}}\_access.log combined

</VirtualHost>

```

## Pasos para virtualizar entorno productivo de una aplicación Backend + Front-end

```

<VirtualHost \*:80>

ServerName {{ TUDOMINIO }}.com
ServerAlias www.{{ TUDOMINIO }}.com
ServerAdmin admin@{{ TUDOMINIO }}.com

DocumentRoot /var/www/html/{{NOMBRE DE LA CARPETA DE TU  FRONT-END}}/dist

<Directory /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO FRONT-END}}/dist>
Options -Indexes +FollowSymLinks
AllowOverride All
</Directory>

Alias /{{ALIAS}} /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO BACKEND}}/public

<Directory /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO BACKEND}}/public>
Options -Indexes +FollowSymLinks
AllowOverride All
</Directory>

ErrorLog ${APACHE_LOG_DIR}/{{NOMBRE DEL PROYECTO}}\_error.log
CustomLog ${APACHE_LOG_DIR}/{{NOMBRE DEL PROYECTO}}\_access.log combined

</VirtualHost>

```

\*Nota

```
La línea 'DocumentRoot' especifica la ruta del directorio raíz para el front-end del proyecto web.

La línea 'Alias' especifica un alias para una ruta específica en el servidor, en este caso, la ruta /{{ALIAS}} se vinculará a la carpeta /var/www/html/{{NOMBRE DE LA CARPETA DE TU PROYECTO BACKEND}}/public. Esto significa que cualquier solicitud a la ruta /{{ALIAS}} se dirigirá a la carpeta public del proyecto back-end.
```

2. Guardar el archivo 000-default.conf con permisos de administrador.

3. Activar el proyecto con el comando:

```
sudo a2ensite {{ NOMBRE DEL PROYECTO }}
```

4. Editar el archivo /etc/hosts y agregar la dirección IP local y el nombre del proyecto.

```
#Web-Sites

127.0.0.1   {{TUDOMINIO}}.com
```

5. Reiniciar Apache con el comando:

```
sudo service apache2 restart
```

6. Dar permisos de ROOT dentro del proyecto ejecutando los siguientes comandos:

```
/* SOLO PARA ENTORNO LARAVEL*/

chmod 777 -R storage/
chmod 777 -R bootstrap/

/*CUALQUIER ENTORNO*/

sudo chmod -R 777 /ruta/a/la/carpeta
```

1. Despues de que instalaras todas las dependencias necesarias ejecutar :

```
npm run dev
```

8. Abrir en un navegador tu dominio registrado y tendrias todo listo.

```
{{TUDOMINIO}}.com
```
