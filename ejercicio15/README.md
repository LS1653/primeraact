static void processData(uint8_t *pData, uint8_t size, uint8_t *res)
{

    uint8_t sum = 0;
    for (int i = 0; i < size; i++)
    {
        sum = sum + (pData[i] - 0x30);
    }
    *res = sum;
}

void task1()
{
    enum class Task1States    {
        INIT,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::INIT;
    static uint8_t rxData[5];
    static uint8_t dataCounter = 0;

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
        // evento 1:        if (Serial.available() > 0)
        {
            rxData[dataCounter] = Serial.read();
            dataCounter++;
            if (dataCounter == 5)
            {
                uint8_t result = 0;
                processData(rxData, dataCounter, &result);
                dataCounter = 0;
                Serial.print("result: ");
                Serial.print(result);
                Serial.print('\n');
            }
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

Qué hace, cómo funciona y qué necesitas para probarlo.
a) El programa usa una máquina de estados que espera recibir datos a través del puerto serial. Una vez que se reciben 5 bytes, se procesan y se calcula una suma basada en los valores recibidos. El resultado de esta suma se envía de vuelta por el puerto serial.

b) 
Recepción de Datos:
Cada vez que se recibe un byte por el puerto serial, se almacena en el arreglo rxData.
Cuando se han recibido 5 bytes, estos se procesan en la función processData.

Procesamiento:
La función processData convierte cada byte de rxData de su valor ASCII a su valor numérico y luego los suma.
El resultado de la suma se almacena en la variable result.

Salida:
El resultado final se imprime por el puerto serial.

c)
Hardware:
Una placa compatible con Arduino o cualquier microcontrolador que pueda ejecutar código basado en el IDE de Arduino.
Un cable USB para conectar la placa al computador.

Software:
El IDE de Arduino instalado en tu computadora.
El código cargado en la placa a través del IDE de Arduino.

Pruebas:
Abre el monitor serial en el IDE de Arduino configurado a 115200 baudios.
Envía secuencias de 5 caracteres numéricos ASCII (como "12345", "67890", etc.).
El resultado de la suma de estos números debería aparecer en el monitor serial.

¿Por qué es necesario declarar rxData static? y si no es static ¿Qué pasa? ESTO ES IMPORTANTE, MUCHO.
Es necesesario para que durante el ciclo no pierda los valores que va adquiriendo a medida que se repite dicho ciclo, si no fuera static lo que sucederia es que cada vez que vuelva a empezar el ciclo el valor de rxData se reestableceria al que tenia en su inicialización, osea se perderian los datos que se estaban guardando antes del reinicio del ciclo.

dataCounter se define static y se inicializa en 0. Cada vez que se ingrese a la función loop dataCounter se inicializa a 0? ¿Por qué es necesario declararlo static?
No, no se volveria a inicializar en 0 debido a la funcionalidad de static que mantiene los cambios de la variable incluso aunque pase un ciclo, osea dataCounter se inicializara con el valor que tenia guardado antes de repetirse el ciclo, es por eso mismo que es necesario que se defina como static para que no se pierda o se reinicie la información.

Observa que el nombre del arreglo corresponde a la dirección del primer elemento del arreglo. Por tanto, usar en una expresión el nombre rxData (sin el operador []) equivale a &rxData[0].

En la expresión `sum = sum + (pData[i] - 0x30);` observa que puedes usar el puntero pData para indexar cada elemento del arreglo mediante el operador [].

Finalmente, la constante `0x30` en `(pData[i] - 0x30)` ¿Por qué es necesaria?
0x30 se usa para convertir un carácter numérico ASCII a su valor numérico entero.
Ejemplo: Si se recibe '5' (valor ASCII 53), restando 0x30 se obtiene 5, que es el valor entero deseado.
Su propósito facilita el procesamiento de caracteres numéricos recibidos como datos seriales, permitiendo que el código los trate como enteros y realice operaciones matemáticas con ellos.