-wsl #para activar linux
-source venv/bin/activate # para activar el env

pip install -r requirements.txt para reproducir el entorno
será que esta aprovechando que ahora tiene más espacio para información que es muy similar?

Para pre procesar y procesar los datos se sigue el siguiente pipeline:

1. primer_preprocesamiento.ipynb: toma las partidas que tienen tiempo entre 60 y 300 con elo mayor a 1000 y que no sean bots y las divide en elo medio y alto (2000)
Salida: "../data/2026_1_preproc_[mid/high]_elo.pgn.zst"

2. segundo_preprocesamiento.ipybn: une las cuatro salidas del paso 1 y devuelve 3 sets sin leak de jugadores (train, val y test), sin partidas con menos de 51 plies y balanceados. 
Salida: "/data/generado/noleak/[train,val,test].pgn"

3. tercer_preprocesamiento.ipybn: toma las salidas del paso 2 y obtiene la info que voy a utilizar para entrenar (plies) y lo comprime
Salida: "/data/generado/noleak/[train,val,test]_[ultimoply].npz"

Alpha 0 lila 0

para emdir bien abril de 2019  evitar la pandemia. gambito de dama, mediados 2018 a mediados 2019. Separar por elo primero y después limpiar. ver la puntuación.


reducir tamaño del espacio latente. me conviene overfitee.

ahora mismo tu autoencoder tiene bastante capacidad para aprender una representación que reconstruya bien prácticamente 
cualquier posición. Si querés usar la loss como indicador de "qué tan parecida es una posición a las posiciones de Elo alto", 
necesitás que el modelo tenga cierta capacidad limitada y que esa capacidad esté especialmente aprovechada para las estructuras
 que aparecen en el dataset de Elo alto.

"¿La capacidad de reconstrucción del autoencoder contiene información sobre el nivel de Elo?"


Interesante para probar más adelante: 
 * REPRESENTACIÓN:

    * DONE Hacer que las posiciones sean un tensor de 13 x 8 x 8 en vez de 832. Esto permite usar Redes Convolucionales (ResNets, por ejemplo),
    que son el estándar de oro para procesar tableros (AlphaZero y Stockfish NNUE usan arquitecturas que entienden la grilla 8x8).
    * Arquitectura NNUE


 * MODELO: 

    * DONE Probar diferentes normalizaciones: estandarizacion (x-media/sigma) normalizacion minmax o robust scaling
    * DONE Probar diferentes perdidas (especialmente para evitar que el modelo prediga el promedio): MAE o Huber loss (smooth L1)
    * DONE Rebalanceo del dataset Under samplear elos medianos y oversamplear extremos


* BENCHMARK:
   * INVESTIGAR El modelo antitrampas de la FIDE utiliza el "Intrinsic Performance Rating" (IPR), usa toda la partida y compara con motor stockfish
   * DONE INVESTIGAR Maia Chess

Para analizar: 
 
 * DONE Data loader para cargar datos
 * Probar más cantidad de plies, ver hasta el 80 (enfasis en 60) EL PROBLEMA DE ESTO ES QUE TAL VEZ SON DIFERENTES LAS PERSONAS QUE LLEGAN A PARTIDAS MÁS LARGAS QUE LAS QUE NO LLEGAN
 
 * DONE Usar solo partidas de rapid

 * DONE Obtener un predictor en base a los AUTO-encoders que recreaban posiciones tambien como benchmark

 * DONE Probar a hacer sobre los ensembles lo de rotarlo
 * DONE Probar a analizar solo el elo de un jugador por ejemplo impares y solo el elo blanco

 * DONE equilibrar el dataset

 * Tal vez deberia agregar en el preprocesamiento que se fije que tengan más de x cantidad de plies.

 * El hecho de que solo me fije en partidas que lleguen hasta por ejemplo el ply 51 ineycta un bias en las partidas que veo.

 * Analizar la diferencia entre incluir y no incluir bots en la data. Tal vez puedo demostrar que no es algo positivo


Aclaraciones:
 * Entrenar el modelo en un ply especifico soluciona dos problemas: tener que pasarle al modelo en que ply esta y el ruido que genera la 
 diferencia entre las diferentes etapas de una partida (apertura, medio y end game)

Tesis:
* Todos los lugares que tengan escrito JUSTIFICAR en el latex estaria bueno poder argumentarlos. Ya sea con citas o verificar lo que digo
* Todos los lugares que tengan escrito CAMBIAR REDACCIÓN son cosas que no estoy convencido como estan redactadas
* Todos los lugares que tengan escrito CITAR es porque tendria que citar un paper o algo para justificar lo que estoy afirmando
* Texto puesto en negrita TIENE QUE SER BORRADO, son cosas que me dejo anotadas para preguntar o para recordar

* Lichess utiliza Glicko-2 Un jugador nuevo comienza aproximadamente en: 1500 ± 1000. El 1500 es la estimación inicial de rating, mientras que el ±1000 representa una incertidumbre enorme. Por eso durante las primeras partidas el rating puede cambiar cientos de puntos.

* "Límites de la información puramente posicional", el MLP, la CNN 2D y la CNN 3D convergen al mismo error (~190 MAE) debido a la limitación
 de observar solo 4 plies (tanto en 1 ply como en 4).

* el elo no es la mejor medición de la habilidad del jugador, pues sufre de problemas como inflación. De todas formas, como el elo la forma más común
utilizada para emparejar jugadores con similares niveles, buscamos entender las imperfecciones del sistema utilizando modelos. "How good you are as a chess player should only be related to winning percentage. Nothing else.Aat the end of the day, you don't win a tournament because you made more correct moves. You win a tournament because you win games. If you play dubious gambits, unsound sacrifices, if you're a queen down and your opponent loses on time, guess what: if you win, nothing else matters."

* TAL VEZ SE PUEDE DECIR que como las convoluciones 3D no funcionaron mejor que las 2D, entonces el ajedrez no es un entorno de transformaciones espaciales continuas (como el movimiento de una cámara), sino un sistema de estados discretos

* Una fuente de ruido es que unos jugador de elo alto puede no tomarse en serio una partida

* En la nube de la izquierda (Elo bajo): El modelo tiende a sobreestimar a los peores jugadores (predice 1300-1500 para jugadores de 1100). ¿Por qué? Porque en las posiciones caóticas, el modelo está inseguro. Matemáticamente, cuando un modelo basado en MSE no está seguro, la jugada más segura para minimizar el error es apostar al promedio. Además, ajedrecísticamente, un jugador de 1100 y uno de 1300 cometen errores tácticos muy similares; sus tableros son casi indistinguibles.

* En la nube de la derecha (Elo súper alto): Ocurre lo opuesto. El modelo subestima a la súper-élite (predice 2600 para jugadores de 3000). Esto pasa porque entre 2600 y 3000, ambos jugadores juegan "perfecto" durante los primeros 40 plies. Al no encontrar errores que delaten el nivel, el modelo vuelve a apostar al promedio de la élite para no arriesgarse.


https://www.researchgate.net/publication/221606300_Intrinsic_Chess_Ratings


Lo que hacen en maia para restringir los datos de entrada
"Within each game in a test set, we
discard the first 10 ply (a single move made by one player is one
“ply”) to ignore mostmemorizedopeningmoves,andwediscardany
move where the player had less than 30 seconds to complete the
rest of the game (to avoid situations where players are making ran
dommoves).After these restrictions, each test set contains roughly
500,000 positions each."

segun maia 2: "A difference of 200 points in chess rating systems roughly equates to a 75%
win rate for the higher-rated player"

Maia no va en el capítulo de "Resultados y Comparaciones", va en el "Estado del Arte" (Marco Teórico).
 Citas a Maia (McIlroy-Young et al.) para fundamentar que el estilo humano es predecible y está estrictamente 
 atado al Elo. Maia demostró que los jugadores de 1500 cometen errores estructuralmente distintos a los de 1900.

Tu tesis toma esa premisa validada por Maia y da el siguiente paso: investigar si esos errores estructurales dejan una "huella digital" en la posición estática del tablero que una red neuronal convolucional pueda rastrear sin necesidad de ver el movimiento en sí.



Cosas que preguntar:

Cosas que decir:


To do list:

   1. En vez de predecir el promedio de elo de la partida, juntar partidas de un jugador y ahi intentar predecir su elo... ya de por si estoy descartando una cantidad absurda de datos pero se podría probar, pero yo lo dejaria para más adelante
   
   2. DONE Hacer el benchmark del autoencoder pero para eso necesito decidir que hacer con los datos: necesito entrenar con partidas de elo alto y que sean rapid, pareciera que en elichess elimino muchisimas (de 16k me quedo con 1k)

   3. En el plot de siempre diferenciar apertura, diferencia de elo entre jugadores, y pensar otras variables (para ello hay que modificar el dataset)

   4. Revisar clasificacion en vez de regresion. Tmb revisar intentar predecir no exacto el numero sino el elo +- 100 puntos por ejemplo (y cambiar la loss en base a eso)

   5. Inspirado en maia: Tal vez no solo testear el elo sino quien va a ganar le da más capacidad de comprensión al modelo
   
   6. Agregar otro tipo de data: No estoy agregando quien tiene el turno, y los derechos de enroque y peon al paso.
      * Agregar al modelo casillas atacadas por cada jugador
      * Agregar conteo explicito de material
      * Agregar antes de la ultima capa fully connected el centipawnloss calculado. (Medio trampa?)

   7. DONE Crear un propio benchmark usando randomforest

   8. investigar balanceo automatico de scikit learn

   9. SVM dual

   10. probar si realmente 13 aporta o con 12 esta bien

   11. obtener la distribución de elos de la base que saque

