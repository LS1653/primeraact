void task1()
{
    enum class Task1States    {
        INIT,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::INIT;

    switch (task1State)
    {
    case Task1States::INIT:
    {
        Serial.begin(115200);
        task1State = Task1States::WAIT_DATA;
        break;
    }

    case Task1States::WAIT_DATA:
    {
        // evento 1:        // Ha llegado al menos un dato por el puerto serial? 
          if (Serial.available() > 0)
        {
            Serial.read();
            Serial.print("Hola computador\n");
        }
        break;
    }

    default:
    {
        break;
    }
    }
}

void setup()
{
    task1();
}

void loop()
{
    task1();
}

----------------------------------------------------------------------------

Nota que al final del mensaje hay un 0a ¿A qué letra corresponde?
Según lo que yo entiendo 0a le corresponde al comando break line "/n" osea 0a es la forma de expresar este comando en el ScriptCommunicator.

Analiza el programa. ¿Por qué enviaste la letra con el botón send? ¿Qué evento verifica si ha llegado algo por el puerto serial?
a.Se envia una letra debido a que en el programa esta la función serial.read(), el cual lo que hace es esperar a que el usuario le ingrese algún argumento en este caso mandamos la letra s, pero podria haber sido cualquier letra, número o caracter, la cuaestion es que se envia para poder hacer que el programa continue, ya que si no se envia nada el programa no continuaria.
b.El evento que verifica si ha llegado algo por el evento serial es el evento 1  if (Serial.available() > 0) en el cual se entiende que si no se ha llegado al puerto serial significa que el Serial.available() es igual a 0.

Analiza los números que se ven debajo de las letras. Nota que luego de la r, abajo, hay un número. ¿Qué es ese número?
El enucniado de esta pregunta no me parecio muy clara al respecto a que se refiere por lo que respondere segun lo que creo que se refiere, si el enunciado se refiere a la tabla que se nos paso, lo que hay luego de la r es la t, letra la cual se representa con el número 73 en Hx (Hexadesimal), si no se refiere a esto, la otra opción es lo que se muestra en en el ScriptCommunicator "Hola computador/n" en el cual lo que sigue despues de la r es el /n que el número que tiene debajo es 0a que significa salto de linea o break line cosa ya antes mencionada. Si estas dos respuestas no responden a la prengunta no estoy seguro a lo que me pide en realidad esta pregunta.

¿Qué relación encuentras entre las letras y los números?
Su relación radica en que cada letra le corresponde un número, debido a por ejemplo 72 es la forma de decir r en hexadecimal.

¿Qué es el 0a al final del mensaje y para qué crees que sirva?
0a es la forma de repersentar /n  que significa salto de linea o break line.


