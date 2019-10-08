# PiedraPapelTijera
Ejercicio del juego Piedra, Papel o Tijera de Catalinita

<img id="img1" src="files/img/Catalinita_piedra_papel_o_tijera_pelicula.png">](https://youtu.be/Bt_0hJaireo) <br> <br>

## Catalinita juega a "piedra, papel o tijera"
No pierdas la oportunidad de jugar con Catalinita a "piedra, papel o tijera". A ver cuántas veces te gana.

¡Mira el vídeo y verás cómo se enfada cuando pierde!

[![Catalinita](./Catalinita_pptijera.png)](https://youtu.be/ntjYxE4nAJ0)

#### Poner en marcha el programa _Catalinita\_piedra\_papel\_o\_tijera.js_
Este programa es un poco más difícil de poner en marcha que los anteriores, pero no imposible.<br>
Para agilizar la conversación con Catalinita, los textos que ella verbaliza han sido generados previamente.<br>
Para que Catalinita pueda jugar contigo a "piedra, papel o tijera" tienes que hacer seis cosas:
1) Descargar todos los ficheros de la carpeta [Catalinita_piedra_papel_o_tijera](https://ibm.box.com/s/typqgykshoabfrb7np7z7x95z1kde8xd) a la raspberrypi.
2. Ubicar todos los ficheros descargados en el directorio donde ya estés ejecutando satisfactoriamente el programa _conversation.js_ del TJBot o _Catalinita\_lorito.js_. Posicionarse en ese directorio.
3. Entrenar un modelo de Visual Recognition para enseñar a Catalinita a distinguir entre piedra, papel o tijera:<br>
3.1. Asegurar que la credencial del servicio Visual Recognition de Watson está correctamente introducida en el fichero _config.js_.<br>
3.2. Crear un modelo nuevo específico para este juego:<br>
_curl -X POST -u "apikey:xxx" \
--form "piedra_positive_examples=@piedra.zip" \
--form "papel_positive_examples=@papel.zip" \
--form "tijera_positive_examples=@tijera.zip" \
--form "negative_examples=@negativos.zip" \
--form "name=piepati" \
"https://gateway.watsonplatform.net/visual-recognition/api/v3/classifiers?version=2018-03-19"_<br>
siendo _xxx_ la credencial del servicio Visual Recognition.<br>
Después de un rato largo, el comando devolverá las características del modelo (también llamado clasificación) recién creado. Es importante apuntar el nombre otorgado a esta nueva clasificación. Ese nombre será algo como _piepati\_yyy_, y estará a la derecha del parámetro _"classifier_id":_<br>
3.3. Esperar hasta que el modelo esté listo. Esto es así cuando, al ejecutar:<br>
_curl -X GET -u "apikey:xxx" "https://gateway.watsonplatform.net/visual-recognition/api/v3/classifiers/piepati_yyy?version=2018-03-19"_<br>
el parámetro _"status":_ ofrece el valor _ready_.
4. Modificar la librería _node\_modules/tjbot/lib/tjbot.js_ para que el modelo de Visual Recognition que use Catalinita al jugar sea el recién entrenado, para ello:<br>
4.1. Buscar dentro de la librería la función _TJBot.prototype.recognizeObjectsInPhoto_. (Está alrededor del número de línea 920.)<br>
4.2. Dentro de esa función, en el párrafo donde se rellena la variable _params_, localizar la línea:<br>
_images_file: fs.createReadStream(filePath),_<br>
y añadir a continuación lo siguiente (incluyendo la coma final): <br>
_"classifier_id": "piepati_yyy",_<br>
4.3. Guardar los cambios hechos en el fichero y cerrarlo.
5. Instalar el paquete opn:<br>
_npm install opn_<br>
(Este paquete se utiliza para que Catalinita pueda abrir el navegador y mostrar lo que saca ella en cada tirada.)
6. Ejecutar: <br>
_sudo node Catalinita\_piedra\_papel\_o\_tijera.js_<br>
(Debes tener la precaución de cerrar todas las ventanas de las tiradas, cada vez que invoques el programa.)

#### Usar el programa _Catalinita\_piedra\_papel\_o\_tijera.js_
Puedes interactuar con Catalinita diciéndole simplemente "piedra, papel o tijera":
1. Ella te contestará: _venga, saca tú_.
2. Deberás enseñarle tu opción acercando tu mano a la cámara a un palmo de distancia. Para que el modelo entrenado funcione a la perfección, es conveniente que la cámara esté situada a la altura de tu cintura, y que detrás de ti haya una pared blanca, o por lo menos, que no haya nadie más, ni brazos, ni manos, ni piernas, ni caras.
3. Al mismo tiempo que tú sacas, Catalinita por pantalla mostrará lo que saca ella. 
4. Ella evaluará lo que tú has sacado, lo comparará con lo suyo y anunciará quién es el ganador. 

¡Ánimo y que tengas mucha suerte!

#### Más comandos útiles para gestionar la clasificación/modelo de Visual Recognition
Otra forma de extraer el _classifier\_id_ y el _status_ es obteniendo la información de todas las clasificaciones asociadas al servicio Visual Recognition que se esté usando. Para hacer esto usa el comando:<br>
_curl -u "apikey:xxx" "https://gateway.watsonplatform.net/visual-recognition/api/v3/classifiers?verbose=true&version=2018-03-19"_

Para evaluar la eficacia del modelo creado se puede intentar clasificar manualmente una imagen, para ello, basta con ejecutar:<br>
_curl -X POST -u "apikey:xxx" --form "images_file=@www"  --form "classifier_ids=piepati_yyy" "https://gateway.watsonplatform.net/visual-recognition/api/v3/classify?version=2018-03-19"_<br>
siendo _www_ el nombre del fichero jpg, con path completo, que se quiere someter a examen.

Como el número de modelos que se pueden tener en un servicio Visual Recognition es limitado, puede ser interesante borrar modelos que ya no se usen:<br>
_curl -X DELETE -u "apikey:xxx" "https://gateway.watsonplatform.net/visual-recognition/api/v3/classifiers/piepati_yyy?version=2018-03-19"_

#### Problemas comunes al usar el programa _Catalinita\_piedra\_papel\_o\_tijera.js_
Si tienes problemas, échale un ojo a los siguientes puntos:
1. Si dudas de las imágenes que capta tu TJBot, puedes ver las capturas en el directorio /tmp de tu raspberrypi.
2. Si ves que el modelo entrenado con las imágenes proporcionadas por Catalinita no se ajusta a tus necesidades, no dudes en incorporar en los ficheros _.zip_ más imágenes de cada tipo para enseñar a Catalinita a distinguir mejor entre piedra, papel o tijera.
3. Si al ejecutar _sudo node Catalinita\_piedra\_papel\_o\_tijera.js_ obtienes un error del siguiente estilo:<br>
<<<br>
verbose: TJBot initializing LED<br>
verbose: TJBot initializing microphone<br>
verbose: TJBot initializing servo motor on PIN 7<br>
2019-06-08 10:15:55 initInitialise: bind to port 8888 failed (Address already in use)<br>
/home/KKCHU/node_modules/pigpio/pigpio.js:11<br>
    pigpio.gpioInitialise();<br>
           ^<br>
Error: pigpio error -1 in gpioInitialise<br>
    at initializePigpio (/home/KKCHU/node_modules/pigpio/pigpio.js:11:12)<br>
    at new Gpio (/home/KKCHU/node_modules/pigpio/pigpio.js:25:3)<br>
    at TJBot._setupServo (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:304:19)<br>
    at TJBot.<anonymous> (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:71:18)<br>
    at Array.forEach (<anonymous>)<br>
    at new TJBot (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:56:14)<br>
    at Object.<anonymous> (/home/KKCHU/ppt/ppt.js:46:10)<br>
    at Module._compile (internal/modules/cjs/loader.js:654:30)<br>
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:665:10)<br>
    at Module.load (internal/modules/cjs/loader.js:566:32)<br>
    at tryModuleLoad (internal/modules/cjs/loader.js:506:12)<br>
    at Function.Module._load (internal/modules/cjs/loader.js:498:3)<br>
    at Function.Module.runMain (internal/modules/cjs/loader.js:695:10)<br>
    at startup (internal/bootstrap/node.js:201:19)<br>
    at bootstrapNodeJSCore (internal/bootstrap/node.js:516:3)_<br>
\>><br>
cierra todas las ventanas del navegador que tengas abiertas.<br>
Debes tener la precaución de cerrar todas las ventanas de las tiradas, cada vez que invoques el programa.
