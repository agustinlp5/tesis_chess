-wsl #para activar linux
-source venv/bin/activate # para activar el env

pip install -r requirements.txt para reproducir el entorno
será que esta aprovechando que ahora tiene más espacio para información que es muy similar?

Alpha 0 lila 0

para emdir bien abril de 2019  evitar la pandemia. gambito de dama, mediados 2018 a mediados 2019. Separar por elo primero y después limpiar. ver la puntuación.


reducir tamaño del espacio latente. me conviene overfitee.

ahora mismo tu autoencoder tiene bastante capacidad para aprender una representación que reconstruya bien prácticamente 
cualquier posición. Si querés usar la loss como indicador de "qué tan parecida es una posición a las posiciones de Elo alto", 
necesitás que el modelo tenga cierta capacidad limitada y que esa capacidad esté especialmente aprovechada para las estructuras
 que aparecen en el dataset de Elo alto.

"¿La capacidad de reconstrucción del autoencoder contiene información sobre el nivel de Elo?"


gemini sobre la toma de datos:
Guardadas 5000 partidas... (Descartadas por cruces: 4619).Esto no significa que de 5,000 se descartaron 4,619 (lo leíste como "de 500 partidas")
 Significa que el sistema leyó 9,619 partidas en total: guardó 5,000 que cumplían la condición y descartó 4,619 por cruces de jugadores.
 4619/9619 = 48.01% de descarte. el código funcionó matemáticamente perfecto según las proporciones de jugadores que le dimos!
 ¿Por qué el Train quedó con 91,474 partidas? Porque en el ajedrez se necesitan dos personas para generar una partida. 
 Las probabilidades se multiplican al cuadrado.Al dividir a los jugadores en 70% Train, 15% Val y 15% Test, las probabilidades de que una partida
ocurra dentro de un mismo grupo son:
Train vs Train: 0.70 * 0.70 = 0.49. Val vs Val y Test vs Test: 0.15 * 0.15 = 0.0225
Si sumamos estas tres probabilidades válidas obtenemos el $0.535$ total. Por lo tanto, de las partidas que sí se guardan, 
la porción que va a Train es:$$\frac{0.49}{0.535} = 91.5\%$$¡Y el 91.5% de tus 100,000 partidas son exactamente esas 91,474 que obtuviste!


Interesante para probar más adelante: 
 * REPRESENTACIÓN:

    * DONE Hacer que las posiciones sean un tensor de 13 x 8 x 8 en vez de 832. Esto permite usar Redes Convolucionales (ResNets, por ejemplo),
    que son el estándar de oro para procesar tableros (AlphaZero y Stockfish NNUE usan arquitecturas que entienden la grilla 8x8).
    * Arquitectura NNUE
    * Agregar al modelo casillas atacadas por cada jugador
    * Agregar conteo explicito de material
    * Agregar antes de la ultima capa fully connected el centipawnloss calculado. (Medio trampa?)

 * MODELO: 

    * DONE Probar diferentes normalizaciones: estandarizacion (x-media/sigma) normalizacion minmax o robust scaling
    * DONE Probar diferentes perdidas (especialmente para evitar que el modelo prediga el promedio): MAE o Huber loss (smooth L1)
    * Rebalanceo del dataset Under samplear elos medianos y oversamplear extremos


* BENCHMARK:
   * INVESTIGAR El modelo antitrampas de la FIDE utiliza el "Intrinsic Performance Rating" (IPR), usa toda la partida y compara con motor stockfish
   * INVESTIGAR Maia Chess
   * Crear un propio benchmark usando randomforest

Para analizar: 
 * No estoy agregando quien tiene el turno, y los derechos de enroque y peon al paso.
 * DONE Data loader para cargar datos
 * Probar más cantidad de plies, ver hasta el 80 (enfasis en 60)
 * Usar solo las partidas que tienen un tipo de apertura
 * Usar solo partidas de rapid
 * Tal vez no solo testear el elo sino quien va a ganar le da más capacidad de comprensión al modelo
 * Obtener un predictor en base a los AUTO-encoders que recreaban posiciones tambien como benchmark
 * Probar a analizar solo el elo de un jugador por ejemplo impares y solo el elo blanco
 * Clasificacion en vez de regresion
 * Sacar ECO A 00 - 09, B 00 - 09, C 00 - 09, D 00 - 09, E 00 - 09 porque son feas.
 * Promediar multiples partidas


Aclaraciones:
 * Entrenar el modelo en un ply especifico soluciona dos problemas: tener que pasarle al modelo en que ply esta y el ruido que genera la 
 diferencia entre las diferentes etapas de una partida (apertura, medio y end game)

Poner en la tesis:
* "Límites de la información puramente posicional", el MLP, la CNN 2D y la CNN 3D convergen al mismo error (~190 MAE) debido a la limitación
 de observar solo 4 plies (tanto en 1 ply como en 4).
* el elo no es la mejor medición de la habilidad del jugador, pues sufre de problemas como inflación. De todas formas, como el elo la forma más común
utilizada para emparejar jugadores con similares niveles, buscamos entender las imperfecciones del sistema utilizando modelos. "How good you are as a chess player should only be related to winning percentage. Nothing else. Even if we threw out the laughable quality of Lichess' engine analysis and used strong analysis it would still be fallible. Even if we threw that out and God handed us thirty-two man tablebases, elo would be still superior as a rating system because at the end of the day, you don't win a tournament because you made more correct moves. You win a tournament because you win games. If you play dubious gambits, unsound sacrifices, if you're a queen down and your opponent loses on time, guess what: if you win, nothing else matters."


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

aia no va en el capítulo de "Resultados y Comparaciones", va en el "Estado del Arte" (Marco Teórico).
 Citas a Maia (McIlroy-Young et al.) para fundamentar que el estilo humano es predecible y está estrictamente 
 atado al Elo. Maia demostró que los jugadores de 1500 cometen errores estructuralmente distintos a los de 1900.
  Tu tesis toma esa premisa validada por Maia y da el siguiente paso: investigar si esos errores estructurales dejan
   una "huella digital" en la posición estática del tablero que una red neuronal convolucional pueda rastrear sin 
   necesidad de ver el movimiento en sí.


   