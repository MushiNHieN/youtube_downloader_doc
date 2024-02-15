# Descarga única

Ahora iremos con las funciones de descarga. Estas son más complejas, así que presta atención:

```python
def dl_single(path, link):

    if mp3_checked.get() == 'on':
        video = YouTube(link)
        stream = video.streams.get_audio_only()
        complete_label.configure(
            text=f'Descargando: "{stream.title}"...', text_color='cyan')
        try:
            stream.download(output_path=path,
                            filename=f'{replace_invalid_chars(stream.title)}.mp3', timeout=20)
            complete_label.configure(
                text=f'Descarga de "{stream.title}" completada.', text_color='#07fc03')
        except Exception as e:
            complete_label.configure(
                text=f"'{stream.title}' {e}.", text_color='orange')
  

    if mp4_checked.get() == 'on':
        video = YouTube(link)
        stream = video.streams.filter(
            progressive=True).get_highest_resolution()
        complete_label.configure(
            text=f'Descargando: "{stream.title}"...', text_color='cyan')
        try:
            stream.download(output_path=path,
                            filename=f'{replace_invalid_chars(stream.title)}.mp4', timeout=20)
            complete_label.configure(
                text=f'Descarga de "{stream.title}" completada.', text_color='#07fc03')
        except Exception as e:
            complete_label.configure(
                text=f'"{stream.title}" {e}.', text_color='orange')
  

```

La función aceptará como parámetros un `path`, que será el directorio de descarga y un `link`, que será el link de YouTube que se descargará.

### .mp3

Primeramente comprobaremos si el checkbox de mp3 `mp3_checked` está 'on' con el método `.get()`.

Crearemos una instancia de Youtube `YouTube(link)` con el `link` como parámetro y la guardaremos en la variable `video`.

Después accederemos a los `streams` del `video` y llamaremos al método `get_audio_only()` para coger solo el audio.

Ahora configuraremos nuestro `complete_label` con el texto que indica que el vídeo se está descargando. Podemos acceder al título del vídeo con `stream.title`. Le pondremos un `text_color` cyan.

Dentro de un bloque "try-except", dentro del `try` le diremos que intente hacer la descarga de `stream`, poniéndole en el parámetro `output_path` la variable `path` y como `filename` una "f-string" con lo que retorne la función `replace_invalid_chars()` con `stream.title` seguido de ".mp3".

Luego configuramos `complete_label` con el texto de descarga completada y con un `text_color` `#07fc03`, que es un verde vistoso. Este texto aparecerá cuando la descarga se haya completado.

Dentro del `except` configuraremos `complete_label` para que muestre el error con un `text_color` naranja. Esto se mostrará si ha habido algún error en la descarga.

### .mp4

Para la parte en la que se descarga el .mp4 haremos lo mismo. La única diferencia reside en la variable `stream`.

Accederemos a los `streams` de `video` y accederemos al método `filter()` y meteremos como parámetro `progressive=True`. Esto hará que coja los streams que son "progressive". Estos tienen las pistas de audio y vídeo unidas, lo que nos ahorra trabajo. Por contra solo podemos acceder a una calidad de 720p como máximo.

Luego accederemos al método `.get_highest_resolution()`, lo que hará que coja la máxima resolución disponible (720p como hemos dicho antes).

Guardaremos el `filename` de la misma forma, cambiando la extensión a ".mp4".

Con esto ya tendríamos la función que descarga un único vídeo o canción. Estamos listos para seguir con la función que [descarga listas de reproducción](descarga_lista.md).
