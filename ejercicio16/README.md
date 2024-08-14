Qué crees que ocurre cuando:

¿Qué pasa cuando hago un [Serial.available()](https://www.arduino.cc/reference/en/language/functions/communication/serial/available/)?
Cuando se usa Serial.available(), esta función devuelve el número de bytes que están disponibles en el buffer de recepción del puerto serial. En otras palabras, muestra cuántos bytes de datos han llegado y están listos para ser leídos.

Si no hay datos disponibles, Serial.available() devolverá 0.
Si hay, por ejemplo, 3 bytes de datos en el buffer, Serial.available() devolverá 3.

¿Qué pasa cuando hago un [Serial.read()](https://www.arduino.cc/reference/en/language/functions/communication/serial/read/)?

erial.read() lee el próximo byte disponible en el buffer de recepción del puerto serial y lo devuelve como un valor de tipo int. Este valor es el código ASCII del carácter recibido (o directamente el valor del byte si no es un carácter ASCII).

Después de leer un byte con Serial.read(), ese byte se elimina del buffer, y la función devuelve su valor.
Si el buffer estaba lleno con, por ejemplo, 5 bytes, después de un Serial.read() el buffer tendrá 4 bytes restantes.

¿Qué pasa cuando hago un Serial.read() y no hay nada en el buffer de recepción?

Al llamar a Serial.read() cuando no hay datos en el buffer de recepción (es decir, cuando Serial.available() es 0), la función devolverá -1. Esto permite saber que no había datos disponibles para leer.

Un patrón común al trabajar con el puerto serial es este:

**`if**(Serial.available() > 0){
    int dataRx = Serial.read()
}`

Este patrón se utiliza para asegurarse de que solo se intenta leer un byte cuando hay datos disponibles en el buffer. Así se evita leer cuando no hay datos, lo que podría provocar que Serial.read() devuelva -1.

¿Cuántos datos lee Serial.read()?

Serial.read() lee un solo byte del buffer de recepción cada vez que se llama. Devuelve ese byte como un entero (int), que es el valor ASCII del carácter recibido (o el valor del byte si es un valor binario).

¿Y si quiero leer más de un dato? No olvides que no se pueden leer más datos de los disponibles en el buffer de recepción porque no hay más datos que los que tenga allí.

Si se quier leer más de un dato, necesitas llamar a Serial.read() repetidamente, por ejemplo, dentro de un bucle. Pero es fundamental asegurarse de que hay suficientes datos en el buffer llamando a Serial.available() antes de leer.

Ejemplo: while (Serial.available() > 0) {
           int dataRx = Serial.read();
           // Procesa dataRx
        }


¿Qué pasa si te envían datos por serial y se te olvida llamar Serial.read()?

Si se envían datos por serial y no llamas a Serial.read(), los datos se acumularán en el buffer de recepción del puerto serial. El buffer tiene una capacidad limitada (generalmente 64 bytes en Arduino), por lo que si no lo lees y sigue recibiendo datos, eventualmente el buffer se llenará.

Buffer lleno: Una vez que el buffer se llena, los nuevos datos que lleguen se perderán porque no hay espacio para almacenarlos.
Perder datos: Esto significa que los datos recibidos después de que el buffer se haya llenado se descartarán, y no podrás recuperarlos ni leerlos.