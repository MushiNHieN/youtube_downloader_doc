# Test final

Ya hemos terminado de escribir todo el código del programa. ¡Comprueba que lo tienes todo bien!

## Código completo

```python
from pytube import YouTube, Playlist
import customtkinter as ctk
import re
import threading
import os


# Utilidades
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


def open_file_dialog():
    global file_path
    file_path = ctk.filedialog.askdirectory()
    if file_path:
        output_label.configure(text=f'La descarga se guardará en: {file_path}')


def open_folder(folder):
    os.startfile(folder)


# Descargas
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


# Interfaz
root = ctk.CTk()
root.title('Youtube Downloader')
root.geometry('700x450')
root.iconbitmap('youtube.ico')


link_entry = ctk.CTkEntry(root,
                          placeholder_text='Introduce un link o una lista de reproducción de YouTube',
                          width=600)
link_entry.pack(pady=30)
link_label = ctk.CTkLabel(root, text='', text_color='red')
link_label.pack()

output_button = ctk.CTkButton(
    root, text='Seleccionar directorio', command=open_file_dialog)
output_button.pack()

output_label = ctk.CTkLabel(root, text='', text_color='cyan')
output_label.pack()

mp3_checked = ctk.StringVar(value="off")
mp4_checked = ctk.StringVar(value="off")

mp3_checkbox = ctk.CTkCheckBox(
    master=root, text='.mp3', variable=mp3_checked,  onvalue='on', offvalue='off')
mp3_checkbox.pack(pady=10)

mp4_checkbox = ctk.CTkCheckBox(
    master=root, text='.mp4', variable=mp4_checked,  onvalue='on', offvalue='off')
mp4_checkbox.pack(pady=10)

download_button = ctk.CTkButton(root, text='Descargar', command=lambda: threading.Thread(
    target=download, args=(file_path, link_entry.get())).start())
download_button.pack(pady=10)

playlist_label = ctk.CTkLabel(root, text='', wraplength=400, text_color='cyan')
playlist_label.pack()

complete_label = ctk.CTkLabel(root, text='', wraplength=400)
complete_label.pack()

count_label = ctk.CTkLabel(root, text='', text_color='cyan')
count_label.pack()

open_folder_button = ctk.CTkButton(root, text='Abrir descargas', command=lambda: threading.Thread(
    target=open_folder, args=(file_path,)).start())


root.mainloop()

```

Ahora puedes comprobar que todo funcione correctamente. Ejecuta `youtube_downloader.py` y prueba todas las funciones.
