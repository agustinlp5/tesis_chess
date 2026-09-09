-wsl #para activar linux
-source venv/bin/activate # para activar el env

pip install -r requirements.txt para reproducir el entorno
será que esta aprovechando que ahora tiene más espacio para información que es muy similar?

Base:

Epoch 44/50
329/329 ━━━━━━━━━━━━━━━━━━━━ 4s 12ms/step - acc: 0.9866 - loss: 0.0366 - precision: 0.8704 - recall: 0.7590 - val_acc: 0.9859 - val_loss: 0.0390 - val_precision: 0.8574 - val_recall: 0.7515

weighted_bce con 5

Epoch 36/50
329/329 ━━━━━━━━━━━━━━━━━━━━ 7s 16ms/step - acc: 0.9790 - loss: 0.0930 - precision: 0.6667 - recall: 0.8913 - val_acc: 0.9780 - val_loss: 0.1008 - val_precision: 0.6550 - val_recall: 0.8821

weighted_bce con 2

Epoch 41/50
329/329 ━━━━━━━━━━━━━━━━━━━━ 4s 11ms/step - acc: 0.9857 - loss: 0.0550 - precision: 0.8008 - recall: 0.8261 - val_acc: 0.9849 - val_loss: 0.0588 - val_precision: 0.7900 - val_recall: 0.8170

sacar la capa de activación de la capa del medio.
Alpha 0 lila 0

para emdir bien abril de 2019  evitar la pandemia. gambito de dama, mediados 2018 a mediados 2019. Separar por elo primero y después limpiar. ver la puntuación.

reducir a 180 en vez de 60.

ver como lo recostruye.

escribir de la perdida.

ver lo de la estadística para aparear y eso, lo de entrenar mas de un encoder.

reducir tamaño del espacio latente. me conviene overfitee.

hacer algunas ventanas de 100 o 50.


5000 10000 partidos de validación de cada elo

ver 1 minuto y diferentes tiempos comparar gran maestro con menos tiempo contra un elo mas bajo con mas tiempo

ahora mismo tu autoencoder tiene bastante capacidad para aprender una representación que reconstruya bien prácticamente cualquier posición. Si querés usar la loss como indicador de "qué tan parecida es una posición a las posiciones de Elo alto", necesitás que el modelo tenga cierta capacidad limitada y que esa capacidad esté especialmente aprovechada para las estructuras que aparecen en el dataset de Elo alto.

"¿La capacidad de reconstrucción del autoencoder contiene información sobre el nivel de Elo?"



EL ULTIMO VECTOR DE POSICIONES VACIAS RESULTA QUE EMPEORA EL ENTRENAMIENTO? no solo empeora sino que también hace que no me alcance la ram… voy a seguir con el de 12 en vez del de 13. 

El trece volvi a probarlo y por más de qué le aumente el tamaño latente o lo achique, no hay una mejora en las épocas, siempre predice lo mismo y le devuelve la misma los, como que no mejora

promediar 5 posiciones no me sirvió de mucho.

Me di cuenta que me estaba fijando la posición 20 que es 10 de cada uno, al fijarme la posición 40 que sería 20 de cada uno el modelo funciona peor al menos en el.

Es muy raro porque a veces como que se estanca en una los de 0.07 y no mejora, con un recall altisimo, y otras veces logra bajar, sin cambiar nada

no sé que hacer cuando no cambia la loss, como que no aprende, me paso ya un par de veces. da 0 pasos


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