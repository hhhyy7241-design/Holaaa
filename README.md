![Esta es una imagen](https://github.com/SokyFre2/enlace-Directo/blob/main/assets/Images/IMG_20220710_180342_403.jpg)
# @UploadFreBot
Bot De Telegram : @UploadFreBot , Descargador gratis de contenido desde internet a hacia moodles , nexcloud en cuba

# Comandos En El Bot (Usuarios Nomales)
/start : Inicar Bot , Te Da La INfo
/tutorial : Te Da un tutorial basico de uso del bot q puedes echarle un ojo
/myuser : Obtiene la informacion del usuario q esta usando el bot
/zips : Configura el tamano de las partes comprimidas 7z
/account: COnfigura su cuenta de nube en el bot
/host : Configura el Host Al Cual ba a subir los archivos el bot x ejemplo https://moodle.uclv.edu.cu/ (Moodle o Nexcloud)
/repoid : EN EL caso de las moodles cada nube tiene su repoid q hay q saber extraerlo y configurarsel al bot para poder subir
/cloud : Alterna El tipo de subida a nubes ya sea cloud o moodle , en caso de cloud es nexcloud pero para simplificar se pone cloud
/tokenize_on : Enciende el modo tokenize , se recomienda no usar a no se q disponga de una de las apps oficiales de descarga del bot 
/tokenize_off : Apaga el modo tokenize
/uptype : Configure el modo de subir de moodle ya sea draft , evidence , blog y calendario
/proxy : Configura UN Proxy Para Las Subidas Del Bot , contactar en telegram a @SokyShop para contratar uno
/files : En caso de tener activa el uptype (evidence) este comando le da una lista de archivo q se encuentra en las evidencias de la nube
/delall : En caso de tener activa el uptype (evidence) este comando borra todos los archivos en la lista de evidencia de la nube
/dir : En caso de tener activo cloud configure el directorio base en la nexcloud donde se va a subir los archivos


# Comandos En El Bot (Administrador) 
/adduser : permite un usuario de telegram tener acceso al bot
/banuser : quita acceso al bot de un usuario de telegram
/getdb : Obten la base de datos donde se almacenan la info de los usarios en el bot

# Deploy Directo (Heroku)
[![Heroku Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/SokyFre2/uploadFre)

## Configuración segura y subida por partes

Las credenciales no deben estar en `main.py`. Copia `.env.example` a `.env` y
rellena las variables de entorno antes de arrancar el bot. El token de
Telegram y las contraseñas que estuvieron en versiones anteriores deben
revocarse o cambiarse.

`CHUNK_SIZE_MB` controla el tamaño máximo de cada volumen que se genera para
archivos grandes. El flujo divide el archivo en chunks binarios crudos y después sube cada parte
uno a uno a Moodle. El límite efectivo es el
menor entre `CHUNK_SIZE_MB` y el límite `zips` configurado para la nube.

También se incluye `ChunkedMoodleUploader.py` como capa reutilizable y
`chunk_code.py` para generar y reconstruir el código único de descarga.

### Descargar un código de chunks

El resultado no es una URL HTTP nativa de Moodle: es un código `ETCHUNK1` que
contiene el manifiesto de las partes. Para reconstruir el archivo en otro
equipo:

```bash
pip install -r requirements.txt
python chunk_downloader.py 'ETCHUNK1:...'
```

También se puede indicar otro nombre de salida:

```bash
python chunk_downloader.py 'ETCHUNK1:...' -o archivo_reconstruido.bin
```

El descargador ordena las partes, las descarga en streaming y verifica el
tamaño final antes de terminar.
