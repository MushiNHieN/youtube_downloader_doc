# Creando root

Una vez tengamos importados los módulos que vamos a utilizar, empezaremos creando la interfaz de nuestro programa.

## Root

Para ello, crearemos una instancia de `customtkinter` a la que llamaremos `root`:

```python
root = ctk.CTk()
```

Esta será la base de nuestra interfaz. Ahora llamaremos a la función `mainloop()` de nuestro `root`:

```python
root.mainloop()
```

Esta línea debe de ir siempre abajo de nuestro código.

## Comprobando que funciona

Ahora para comprobar que todo funciona correctamente ejecutaremos nuestro archivo `youtube_downloader.py`. Podemos hacerlo haciendo click en el botón de reproducir de arriba a la derecha en la ventana de VSC o abriendo una terminal y escribiendo:

`python youtube_downloader.py`

Si lo hemos hecho todo bien debería aparecer la ventana de nuestro programa.

![1707838808486](image/root/1707838808486.png)

## Configurando root

A continuación configuraremos el nombre de la ventana:

```
root.title('Youtube Downloader')
```

El tamaño de la misma:

```
root.geometry('700x450')
```

Y el icono de la aplicación:

```
root.iconbitmap('youtube.ico')
```

Para conseguir el icono de YouTube iremos a [https://www.flaticon.es/icon-font-gratis/youtube_6422215?related_id=6422215](https://www.flaticon.es/icon-font-gratis/youtube_6422215?related_id=6422215) y descargaremos el icono en png. 

Después convertiremos la imagen al formato `.ico` en [https://convertio.co/es/png-ico/](https://convertio.co/es/png-ico/). 

Finalmente meteremos el icono en la misma carpeta en la que está nuestro `youtube_downloader.py`. Llamaremos a nuestro icono `youtube.ico`

Hasta el momento nuestro código debería verse así:

```python
from pytube import YouTube, Playlist
import customtkinter as ctk
import re
import threading
import os


root = ctk.CTk()
root.title('Youtube Downloader')
root.geometry('700x450')
root.iconbitmap('youtube.ico')

root.mainloop()
```


Ahora que tenemos listo nuestro `root` vamos a [crear el resto de componentes de nuestra interfaz](componentes.md).
