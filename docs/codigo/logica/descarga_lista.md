# Descarga de lista de reproducción

Crearemos la función encargada de descargar listas de reproducción:

```python
def dl_playlist(path, link):
    playlist = Playlist(link)

    if mp3_checked.get() == 'on':
        for i, video in enumerate(playlist.videos):

            playlist_label.configure(
                text=f'Downloading: {playlist.title} playlist')
            complete_label.configure(
                text=video.title, text_color='cyan')
            count_label.configure(text=f'( {i+1} / {len(playlist.videos)} )')
            try:
                stream = video.streams.get_audio_only()
                stream.download(output_path=f'{path}/{playlist.title}',
                                filename=f'{replace_invalid_chars(stream.title)}.mp3', timeout=20)
            except Exception as e:
                complete_label.configure(
                    text=f'{playlist.title} {e}.', text_color='orange')
                continue

        complete_label.configure(
            text=f'{playlist.title} playlist download complete.', text_color='#07fc03')
        playlist_label.configure(text='')
        count_label.configure(text='')

    if mp4_checked.get() == 'on':
        for i, video in enumerate(playlist.videos):

            playlist_label.configure(
                text=f'Downloading: {playlist.title} playlist')
            complete_label.configure(
                text=video.title, text_color='cyan')
            count_label.configure(text=f'( {i+1} / {len(playlist.videos)} )')
            stream = video.streams.filter(
                progressive=True).get_highest_resolution()

            try:
                stream.download(output_path=f'{path}/{playlist.title}',
                                filename=f'{replace_invalid_chars(stream.title)}.mp4', timeout=20)
            except Exception as e:
                complete_label.configure(
                    text=f'{playlist.title} {e}.', text_color='orange')
                continue

        complete_label.configure(
            text=f'{playlist.title} playlist download complete.', text_color='#07fc03')
        playlist_label.configure(text='')
        count_label.configure(text='')
```

La función aceptará los mismos parámetros que la función de descarga única: `path` y `link`.

Crearemos una instancia de `playlist()` con el link como parámetro y la guardaremos en la variable `playlist`.

### .mp3

Comprobaremos si el checkbox de mp3 está en "on" y, si es así, crearemos un bucle `for` para descargar todos los links de la lista.

Haremos `for i, video in enumerate(playlist.videos)` para poder mostrar por qué vídeo vamos.

Dentro configuraremos `playlist_label` con el título de la lista de reproducción que estamos descargando y `complete_label` con el título del vídeo que se está descargando con un `text_color` cyan.
Configuraremos también `count_label` para que muestre el vídeo por el que vamos `i+1` (porque `i` vale `0` en la primera iteración) y cuántos vídeos tiene la lista en total `len(playlist.videos)`.

Luego crearemos un bloque "try-except". configuraremos `output_path` de forma que sea `path/playlist.title` para que se guarde todo en ese directorio. Después haremos lo mismo que hicimos con la descarga única, solo que después de configurar la etiqueta para mostrar el error, introduciremos un `continue` para que siga con la lista.

Al final configuraremos `complete_label` para que muestre que la lista ha terminado de descargarse.

Configuraremos `playlist_label` para que no muestre nada y haremos lo mismo con `count_label`.

### .mp4

Haremos lo mismo que con la parte de ".mp3", cambiando `.get_audio_only()` por `.filter(progressive=True).get_highest_resolution()`. El resto del código es igual a ".mp3".

Ahora que tenemos tanto la función de descarga única como la de descarga de lista de reproducción, podemos [crear una función que englobe las dos](descarga.md) para nuestro botón de descarga.
