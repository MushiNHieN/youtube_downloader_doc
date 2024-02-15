# Vinculando las funciones a los botones

Aquí es donde entra en acción el módulo `threading`. Como ya hemos visto en la [explicación sobre los módulos](../../primeros_pasos/imports.md), `threading` nos permitirá tener abiertos varios procesos a la vez, lo cual hará que nuestro programa no se congele cuando ejecutemos acciones.

## Seleccionar directorio de descarga

Ahora vincularemos la función `open_file_dialog()` con el botón `output_button`:

```python
output_button = ctk.CTkButton(root, text='Seleccionar directorio', command=open_file_dialog)
```

Como ves esto se hace colocando el parámetro `command` y asignándole la función deseada.

Puedes ejecutar el archivo y comprobar que funciona.

## Botón de descarga

A continuación vincularemos la función `download` con el botón `download_button`:

```python
download_button = ctk.CTkButton(root, text='Descargar', command=lambda: threading.Thread(target=download, args=(file_path, link_entry.get())).start())
```

En `command` meteremos una función `lambda`. En Tkinter, cuando metemos una función en `command` a la cual le metemos argumentos, Tkinter interpreta que debe ejecutar dicha función nada más abrir el programa. `lambda` nos evita eso.

Si quieres saber más sobre `lambda` puedes ir [aquí](https://www.freecodecamp.org/espanol/news/expresiones-lambda-en-python/).

Dentro de `lambda` crearemos un nuevo `thread` con `threading.Thread()`.

 `.Thread()` admite el parámetro `target`, que es la función a la que apuntará el `thread`, y `args`, que son los argumentos de la función `target`. En `args` pondremos `file_path`, que es el directorio donde se guardarán las descargas y `link_entry.get()` para coger el link introducido por el usuario. Así tendríamos los dos parámetros que requiere la función `download()`.

Al final, después del cierre de paréntesis de `.Thread()`, añadiremos `.start()` para indicar que se inicialice el `thread`.

## Botón para abrir directorio

Finalmente, haremos lo mismo con el botón `open_folder_button` y la función `open_folder`:

```python
open_folder_button = ctk.CTkButton(root, text='Abrir descargas', command=lambda: threading.Thread(
    target=open_folder, args=(file_path,)).start())
```

Hay que tener en cuenta que lo que se le pasa como `args` a `.Thread()` es una [tupla](https://ellibrodepython.com/tuplas-python), por lo que si pasamos un solo argumento, hay que ponerle una coma detrás para que lo tome como tal.

Una vez vinculados los botones con sus respectivas funciones, [comprobaremos que todo funciona correctamente](comprobando.md).
