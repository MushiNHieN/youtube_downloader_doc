## Creando youtube_downloader.py

Ahora que tenemos todo listo, crearemos un nuevo archivo con nombre `youtube_downloader.py`.

Editaremos el archivo y al principio de este empezaremos a importar las librerías que necesitaremos.

## Pytube

Empezaremos importando  `pytube` para descargar tanto links simples como listas de reproducción. Para ello escribiremos:

`from pytube import YouTube, Playlist`

## Customtkinter

Ahora importaremos `customtkinter` para crear la interfaz gráfica de nuestro programa:

`import customtkinter as ctk`

"as ctk" quiere decir que importaremos la librería con el alias "ctk", así no tendremos que escribir "customtkinter" cada vez que queramos llamar las funciones del módulo.

## Re

También importaremos el módulo `re` (Regular expressions) para sanear los links y nombres de archivos, ya que los títulos de los vídeos o canciones que descargaremos contienen carácteres no permitidos en Windows para nombres de archivos.

`import re`

## Threading

Este módulo es muy importante. Nos permitirá realizar varios procesos a la vez y, por lo tanto, poder hacer otras tareas en nuestro programa mientras se ejecuta el proceso de descarga. Escribimos:

`import threading`

## Os

Por último, importaremos el módulo `os` para abrir la carpeta en la que hayamos descargado los vídeos o canciones. Importamos así:

`import os`

Ya tenemos todo listo para [escribir el código de nuestro programa](../codigo/interfaz/root.md).
