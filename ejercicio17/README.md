sí se pueden leer 3 datos que han llegado al puerto serial:

**`if**(Serial.available() >= 3){
    int dataRx1 = Serial.read()
    int dataRx2 = Serial.read()
    int dataRx3 = Serial.read()
}`

¿Qué escenarios podría tener en este caso?

**`if**(Serial.available() >= 2){
    int dataRx1 = Serial.read()
    int dataRx2 = Serial.read()
    int dataRx3 = Serial.read()
}`

En este caso, se verifica que hay al menos 2 bytes disponibles en el buffer, pero luego se intenta leer 3 bytes. Aquí, hay varias situaciones posibles:

a)Hay 2 bytes disponibles:
dataRx1 y dataRx2 se leerán correctamente.
Al intentar leer el tercer byte (dataRx3), el buffer estará vacío porque Serial.available() solo garantizó la disponibilidad de 2 bytes. Como resultado, Serial.read() devolverá -1 para dataRx3, lo que indica que no hay más datos disponibles.

b)Hay 3 o más bytes disponibles:
En este caso, el código funcionará bien, y los tres bytes serán leídos correctamente por dataRx1, dataRx2, y dataRx3.

c)Menos de 2 bytes disponibles:
El bloque if no se ejecutará porque Serial.available() será menor que 2.