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

* Cosas que debería poner:
   * Entrenar el modelo en un ply especifico soluciona dos problemas: tener que pasarle al modelo en que ply esta y el ruido que genera la diferencia entre las diferentes etapas de una partida (apertura, medio y end game)
   * Lichess arranca con un rating de 1500
   * Tal vez decir que se intentan caputar las imperfecciones del sistema de elo utilizando los modelos
   * Una fuente de ruido es que unos jugador de elo alto puede no tomarse en serio una partida
   * Según maia 2: "A difference of 200 points in chess rating systems roughly equates to a 75%
   win rate for the higher-rated player"

## Cosas que preguntar:

* Me pongo a full a escribir la tesis o sigo probando cosas?

* Con respecto al Elo, debería poner lo del calculo matematico o no es necesario...

* Si digo que intentar predecir el maximo o el minimo en vez del promedio del elo me daba peor tengo que justificarlo no? pero donde lo pondría? en resultados? o lo menciono en metodologia-variable objetivo y despues pongo los resultados más explicitos en la parte de resultados. Como que al formar parte del analisis exploratorio no son los resultados más importantes de la tesis tonces no se donde ponerlos 

* En las definiciones del marco teórico por ejemplo uso C para decir la cantidad de clases, debería decir lo que es C la primera vez o cada vez que aparece

* Es informal utilizar enumeraciones o items. Hay muchas veces que me dan ganas de utilizarlos, facilitan la lectura y la redacción pero puede ser considerado informal (además de que a la ia le encanta hacer enumeraciones y punteos)

## Cosas que decir:


## To do list:

   1. En vez de predecir el promedio de elo de la partida, juntar partidas de un jugador y ahi intentar predecir su elo... ya de por si estoy descartando una cantidad absurda de datos pero se podría probar, pero yo lo dejaria para más adelante

   2. En el plot de siempre diferenciar apertura, diferencia de elo entre jugadores, y pensar otras variables (para ello hay que modificar el dataset)

   3. Revisar clasificacion en vez de regresion. Tmb revisar intentar predecir no exacto el numero sino el elo +- 100 puntos por ejemplo (y cambiar la loss en base a eso)

   4. Inspirado en maia: Tal vez no solo testear el elo sino quien va a ganar le da más capacidad de comprensión al modelo
   
   5. Agregar otro tipo de data: No estoy agregando quien tiene el turno, y los derechos de enroque y peon al paso.
      * Agregar al modelo casillas atacadas por cada jugador
      * Agregar conteo explicito de material
      * Agregar antes de la ultima capa fully connected el centipawnloss calculado. (Medio trampa?)


   6. Investigar balanceo automatico de scikit learn

   7. SVM dual

   8. Buscar alpha 0 y lila 0 ?

   9. No es necesario porque cite a maia pero podría Analizar la diferencia entre incluir y no incluir bots en la data. Tal vez puedo demostrar que no es algo positivo

   10. probar si realmente 13 aporta o con 12 esta bien

   11. DONE obtener la distribución de elos de la base que saque

   12. Corregir la función de EMD y probar

   13. Probar a aplicar la suavización gaussiana y otro de los métodos?

   14. Revisar que en el marco teórico por ejemplo lo de y hat lo explique la primera vez que lo nombro

   15. Mencionar que el peor caso posible generalmente esta relacionado con partidas en elos elevados con posiciones cuestionables.

   16. Buscar fuentes para el marco teórico?

   17. Citar los lugares de los que saque la explicacion de glicko y de elo: https://www.researchgate.net/publication/221606300_Intrinsic_Chess_Ratings

   18. INTRODUCCION O MARCO TEORICO: Citar a Maia (McIlroy-Young et al.) para fundamentar que el estilo humano es predecible y está estrictamente atado al Elo. Maia demostró que los jugadores de 1500 cometen errores estructuralmente distintos a los de 1900. este trabajo toma esa premisa validada por Maia y da el siguiente paso: investigar si esos errores estructurales dejan una "huella digital" en la posición estática del tablero que una red neuronal convolucional pueda rastrear sin necesidad de ver el movimiento en sí.

   



