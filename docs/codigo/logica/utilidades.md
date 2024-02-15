# Utilidades

Ahora crearemos las funciones que se encargarán de sanitizar los links y abrir el directorio de descarga una vez esta haya finalizado.

Escribiremos estas funciones arriba del código de la interfaz.

## Comprobar link

Crearemos una función que comprobará si el link es de YouTube:

```python
def is_youtube_link(link):
    pattern = r'^(https?://)?(www\.)?(youtube\.com|youtu\.be)/(watch\?v=|embed/|v/|playlist\?list=)?([a-zA-Z0-9_-]{11})(\&.*list=([a-zA-Z0-9_-]+))?$'
    match = re.match(pattern, link)
    return match is not None
```

Recibirá como parámetro el link que hayamos introducido. Luego guardaremos en la variable `pattern` la expresión regular con la que deberá coincidir el link. Si quieres saber más sobre expresiones regulares puedes ir [aquí](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Regular_expressions/Cheatsheet).

Guardaremos lo que nos devuelve la función `.match()` de `re` en la variable `match`. Si `pattern` y `link` coinciden nos devolverá `True`, sino devolverá `False`.

Por último retornaremos `match` si no es `None`.

## Sanitizar link

Algunos navegadores que tienen [Adblock](https://getadblock.com/es/) activado añaden al final de los links de YouTube la cadena "&ab_channel=nombre_del_canal" y esto no lo registra la expresión regular de nuestra función de comprobación de links.

Para solucionar esto crearemos una función que quite esa parte del link si la tiene:

```python
def sanitize_link(link):
    sub = '&ab_channel'
    index = link.find(sub)
    if index != -1:
        return link[:index]
    else:
        return link
```

La función tomará como parámetro el link. Meteremos en la variable `sub` la parte que queremos quitar.

Luego guardaremos en la variable `index` el índice en el que aparece `sub` en `link`.

Si el valor de `index` es diferente a `-1` quiere decir que `sub` se encuentra en `link`. Entonces retornamos `link` quitando la parte que empieza por `sub`. Sino retornamos el `link`, puesto que no contiene `sub`.

## Sanitizar nombre de archivo

Dado que Windows no admite los carácteres `<  > : “ | ? * /` en los nombres de archivo, crearemos una función que los sustituya por un espacio en blanco:

```python
def replace_invalid_chars(filename):
    invalid_chars_regex = r'[<>:"/\\|?*]'
    return re.sub(invalid_chars_regex, ' ', filename)
```

La función toma como parámetro el nombre del archivo. Guardamos en la variable `invalid_chars_regex` la expresión regular que contiene los carácteres prohibidos.

Luego retornamos lo que nos devuelve el método `.sub()` de `re`, sustituyendo los carácteres prohibidos por un espacio en blanco.

## Seleccionar directorio de descarga

Para seleccionar el directorio donde se guardará la descarga escribiremos la siguiente función:

```python
def open_file_dialog():
    global file_path
    file_path = ctk.filedialog.askdirectory()
    if file_path:
        output_label.configure(text=f'La descarga se guardará en: {file_path}')
```

Definiremos la variable global `file_path` y le asignaremos el método `ctk.filedialog.askdirectory()`. Esto abrirá un diálogo con el que podremos seleccionar un directorio.
Luego, si la variable `file_path` tiene valor, configuraremos nuestro `output_label` con el texto que indica dónde se guardará la descarga.

## Abrir directorio de descarga

Por último crearemos la función encargada de abrir el directorio de descarga:

```python
def open_folder(folder):
    os.startfile(folder)
```

Tomará como parámetro el directorio y abrirá el mismo con `os.startfile()`.

Nuestro código debería verse así:

```python
from pytube import YouTube, Playlist
import customtkinter as ctk
import re
import threading
import os

def replace_invalid_chars(filename):
    invalid_chars_regex = r'[<>:"/\\|?*]'
    return re.sub(invalid_chars_regex, ' ', filename)


def is_youtube_link(link):
    pattern = r'^(https?://)?(www\.)?(youtube\.com|youtu\.be)/(watch\?v=|embed/|v/|playlist\?list=)?([a-zA-Z0-9_-]{11})(\&.*list=([a-zA-Z0-9_-]+))?$'
    match = re.match(pattern, link)
    return match is not None


def sanitize_link(link):
    sub = '&ab_channel'
    index = link.find(sub)
    if index != -1:
        return link[:index]
    else:
        return link


def open_folder(folder):
    os.startfile(folder)

def open_file_dialog():
    global file_path
    file_path = ctk.filedialog.askdirectory()
    if file_path:
        output_label.configure(text=f'La descarga se guardará en: {file_path}')

# Interfaz
root = ctk.CTk()
root.title('Youtube Downloader')
root.geometry('700x450')
root.iconbitmap('youtube.ico')


link_entry = ctk.CTkEntry(root,
                          placeholder_text='Introduce un link o una lista de reproducción de YouTube',
                          width=600)
link_entry.pack(pady=30)

output_button = ctk.CTkButton(root, text='Seleccionar directorio')
output_button.pack()

output_label = ctk.CTkLabel(root, text='')


mp3_checked = ctk.StringVar(value="off")
mp4_checked = ctk.StringVar(value="off")

mp3_checkbox = ctk.CTkCheckBox(
    master=root, text='.mp3', variable=mp3_checked,  onvalue='on', offvalue='off')
mp3_checkbox.pack(pady=10)

mp4_checkbox = ctk.CTkCheckBox(
    master=root, text='.mp4', variable=mp4_checked,  onvalue='on', offvalue='off')
mp4_checkbox.pack(pady=10)

download_button = ctk.CTkButton(root, text='Descargar')
download_button.pack(pady=10)

playlist_label = ctk.CTkLabel(root, text='', wraplength=400, text_color='cyan')
playlist_label.pack()

complete_label = ctk.CTkLabel(root, text='', wraplength=400)
complete_label.pack()

root.mainloop()

```

Una vez tengamos esto podremos empezar con las [funciones de descarga](descarga_unica.md).
