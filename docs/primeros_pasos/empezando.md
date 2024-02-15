# Empezando nuestro proyecto

## Creación de entorno virtual con venv

Empezaremos creando un entorno virtual. Esto nos ayudará a la hora de crear el ejecutable, puesto que nuestro programa será mucho más liviano al incluir solo las librerías que hemos instalado.

En este tutorial trabajaremos con [Visual Studio Code](https://code.visualstudio.com).

Crearemos nuestro entorno virtual con [venv](https://docs.python.org/es/3/library/venv.html).

Crearemos una carpeta donde deseemos y la abriremos con VSC. Abriremos una nueva terminal `Ctrl` + `Ñ` e introduciremos el siguiente comando:

`python -m venv env`

Puedes ponerle el nombre que quieras. Nosotros lo llamaremos "env".

Así habremos creado nuestro entorno virtual. A continuación tendremos que activarlo con el comando:

`source env/Scripts/activate`

Recuerda que tendrás que activarlo siempre que abras de nuevo el proyecto.

Ahora estamos listos para [instalar las librerías que necesitaremos](librerias.md).
