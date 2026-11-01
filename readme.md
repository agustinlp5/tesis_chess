## Aclaraciones

- wsl #para activar linux
- source venv/bin/activate # para activar el env

pip install -r requirements.txt para reproducir el entorno
será que esta aprovechando que ahora tiene más espacio para información que es muy similar?

* Para pre procesar y procesar los datos se sigue el siguiente pipeline:

   1. primer_preprocesamiento.ipynb: toma las partidas que tienen tiempo entre 60 y 300 con elo mayor a 1000 y que no sean bots y las divide en elo medio y alto (2000)
   Salida: "../data/2026_1_preproc_[mid/high]_elo.pgn.zst"

   2. segundo_preprocesamiento.ipybn: une las cuatro salidas del paso 1 y devuelve 3 sets sin leak de jugadores (train, val y test), sin partidas con menos de 51 plies y balanceados. 
   Salida: "/data/generado/noleak/[train,val,test].pgn"

   3. tercer_preprocesamiento.ipybn: toma las salidas del paso 2 y obtiene la info que voy a utilizar para entrenar (plies) y lo comprime
   Salida: "/data/generado/noleak/[train,val,test]_[ultimoply].npz"

* Tesis:
   * Todos los lugares que tengan escrito JUSTIFICAR en el latex estaria bueno poder argumentarlos. Ya sea con citas o verificar lo que digo
   * Todos los lugares que tengan escrito CAMBIAR REDACCIÓN son cosas que no estoy convencido como estan redactadas
   * Todos los lugares que tengan escrito CITAR es porque tendria que citar un paper o algo para justificar lo que estoy afirmando
   * Texto puesto en negrita TIENE QUE SER BORRADO, son cosas que me dejo anotadas para preguntar o para recordar
   * Todo codigo va en textit

## Cosas que preguntar:


## Cosas que decir:


## To do list:

   4. Inspirado en maia: Tal vez no solo testear el elo sino quien va a ganar le da más capacidad de comprensión al modelo
   
   8. Buscar alpha 0 y lila 0 ?

   11. INTRODUCCION O MARCO TEORICO: Citar a Maia (McIlroy-Young et al.) para fundamentar que el estilo humano es predecible y está estrictamente atado al Elo. Maia demostró que los jugadores de 1500 cometen errores estructuralmente distintos a los de 1900. este trabajo toma esa premisa validada por Maia y da el siguiente paso: investigar si esos errores estructurales dejan una "huella digital" en la posición estática del tablero que una red neuronal convolucional pueda rastrear sin necesidad de ver el movimiento en sí.

   13. Revisar que en el marco teórico por ejemplo lo de y hat lo explique la primera vez que lo nombro

   14. FALTA MENCIONAR:
      * IMPORTANTE MENCIONAR EN LA INTRODUCCION: se esta usando muy poca información, una sola partida y solo algunos plies. Hablar de como esto demuestra el uso de diferentes arquitecturas en un contexto de poca información o poca entropía de la información.
      * Aclarar que se uso el late fusion en clasificacion, se va a usar ese al final?
      * Agregar a la tabla regresiva EN CLASIFICACION, cuanto dió el modelo

   16. Cosas que puedo poner en la conclusión:

         * WORD chequear la hipotesis de tomar datos de fide o de chess.com
         * WORD En vez de predecir el promedio de elo de la partida, juntar partidas de un jugador y ahi intentar      predecir su elo (Mencionar que investigaciones ya realizadas por el director mostraron que tomar multiples partidas de un jugador reducia ampliamente el error.)
         * Agregar otro tipo de data: No estoy agregando quien tiene el turno, y los derechos de enroque y peon al paso.
            * Agregar al modelo casillas atacadas por cada jugador
            * Agregar conteo explicito de material
            * Agregar antes de la ultima capa fully connected el centipawnloss calculado. (Medio trampa?)
         
   17. Buscar fuentes para el marco teórico?

   23. Buscar todo el codigo y ponerlo en \texttt (reemplazando el textit)

   27. Revisar las citas de Elo

   28. Revisar que las tablas e imagenes esten bien posicionadas

