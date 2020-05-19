# Tarefa 2

# Instalación MariaDB

Para empezar descargaremos MariaDB para la versión de equipo que estemos usando, en mi caso la versión de 64 bits MSI para Windows.

Despues abrimos el archivo y dejamos la primeras opciones por defecto.

![MariaDB1](img/InstalacionMariaDB-1.PNG)

Pondremos la contraseña para el usuario root.

![MariaDB2](img/InstalacionMariaDB-2.PNG)

Dejamos las características por defecto.

![MariaDB3](img/InstalacionMariaDB-3.PNG)

Y instalamos el programa.

![MariaDB4](img/InstalacionMariaDB-4.PNG)

Ahora abrimos las variables del entorno y añadimos la ruta de la carpeta bin del MariaDB en PATH.

![MariaDB5](img/InstalacionMariaDB-5.PNG)

Abrimos el CMD y ponemos el comando para ejecutar MariaDB ``mysql -u root -p``, despues de esto ponemos la contraseña del root, que ya establecimos antes.

![MariaDB6](img/InstalacionMariaDB-6.PNG)

Con esto ya estaría instalado MariDB, ahora haremos una prueba creando una base de datos a ver si funciona bien. 

![MariaDB7](img/InstalacionMariaDB-7.PNG)