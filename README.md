# PiedraPapelTijera
Ejercicio del juego Piedra, Papel o Tijera de Catalinita

Problemas comunes al usar el programa Catalinita_piedra_papel_o_tijera.js

Si tienes problemas, échale un ojo a los siguientes puntos:

    Si dudas de las imágenes que capta tu TJBot, puedes ver las capturas en el directorio /tmp de tu raspberrypi.
    Si ves que el modelo entrenado con las imágenes proporcionadas por Catalinita no se ajusta a tus necesidades, no dudes en incorporar en los ficheros .zip más imágenes de cada tipo para enseñar a Catalinita a distinguir mejor entre piedra, papel o tijera.
    Si al ejecutar sudo node Catalinita_piedra_papel_o_tijera.js obtienes un error del siguiente estilo:
    <<
    verbose: TJBot initializing LED
    verbose: TJBot initializing microphone
    verbose: TJBot initializing servo motor on PIN 7
    2019-06-08 10:15:55 initInitialise: bind to port 8888 failed (Address already in use)
    /home/KKCHU/node_modules/pigpio/pigpio.js:11
    pigpio.gpioInitialise();
    ^
    Error: pigpio error -1 in gpioInitialise
    at initializePigpio (/home/KKCHU/node_modules/pigpio/pigpio.js:11:12)
    at new Gpio (/home/KKCHU/node_modules/pigpio/pigpio.js:25:3)
    at TJBot._setupServo (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:304:19)
    at TJBot. (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:71:18)
    at Array.forEach ()
    at new TJBot (/home/KKCHU/node_modules/tjbot/lib/tjbot.js:56:14)
    at Object. (/home/KKCHU/ppt/ppt.js:46:10)
    at Module._compile (internal/modules/cjs/loader.js:654:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:665:10)
    at Module.load (internal/modules/cjs/loader.js:566:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:506:12)
    at Function.Module.load (internal/modules/cjs/loader.js:498:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:695:10)
    at startup (internal/bootstrap/node.js:201:19)
    at bootstrapNodeJSCore (internal/bootstrap/node.js:516:3)
    >>
    cierra todas las ventanas del navegador que tengas abiertas.
    Debes tener la precaución de cerrar todas las ventanas de las tiradas, cada vez que invoques el programa.

