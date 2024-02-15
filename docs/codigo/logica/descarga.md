# Función de descarga

Ahora uniremos las dos funciones de descarga en una:

```python
def download(path, link):
    open_folder_button.pack_forget()
    link = sanitize_link(link)
    if is_youtube_link(link):
        if path:
            link_label.configure(text='')
            if 'list' in link:
                dl_playlist(path, link)

            else:
                dl_single(path, link)
            open_folder_button.pack()
    else:
        link_label.configure(
            text='Por favor, introduce un link de YouTube válido.', text_color='red')
```

Empezaremos por los parámetros que admite nuestra función: `path` y `link`, como en las anteriores.

Haremos que el botón de abrir el directorio de descarga desaparezca con `.pack_forget()`, por si estuviese ahí a causa de una descarga anterior.

Guardaremos el link sanitizado con nuestra función `sanitize_link()`.

Después comprobaremos si el link es un link de YouTube con la función `is_youtube_link()`, sino configuraremos la etiqueta `link_label` con el texto indicando que se debe de introducir un link de YouTube válido. Utilizaremos un `text_color` red.

Luego comprobaremos si hay un `path`. Si es así, configuraremos `link_label` para que no muestre ningún texto.

Ahora comprobaremos si la palabra "list" está en `link`. Si es así llamaremos a nuestra función `dl_playlist()` con `path` y `link` como parámetros. Sino llamaremos a la función `dl_single()` con los mismos parámetros.

Haremos aparecer el botón de abrir descargas con `.pack()`

Ya tendríamos todo listo para [vincular las funciones con los botones](vinculacion.md).
