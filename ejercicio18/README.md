// Capacidad del buffer de recepción
#define BUFFER_SIZE 32

void task1()
{
    // Estados de la tarea
    enum class Task1States {
        INIT,
        WAIT_DATA
    };
    
    // Estado inicial de la tarea
    static Task1States task1State = Task1States::INIT;
    
    // Buffer de recepción encapsulado
    static uint8_t rxBuffer[BUFFER_SIZE];
    
    // Contador de bytes recibidos
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
        // Verifica si hay datos disponibles en el puerto serial
        while (Serial.available() > 0 && dataCounter < BUFFER_SIZE)
        {
            // Lee el próximo byte del puerto serial y lo almacena en el buffer
            rxBuffer[dataCounter] = Serial.read();
            dataCounter++;
        }

        // Si el buffer se llena, se puede procesar o hacer algo con los datos aquí
        if (dataCounter == BUFFER_SIZE)
        {
            // Procesar los datos del buffer, por ejemplo, imprimir el contenido
            Serial.print("Buffer lleno: ");
            for (uint8_t i = 0; i < BUFFER_SIZE; i++) {
                Serial.print((char)rxBuffer[i]); // Convertir a carácter para imprimir
            }
            Serial.println();

            // Reiniciar el contador si es necesario o continuar con el procesamiento
            dataCounter = 0; // Para reutilizar el buffer, reiniciamos el contador
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
    // Llama a la tarea una vez para inicializarla
    task1();
}

void loop()
{
    // Llama a la tarea repetidamente para manejar la recepción de datos
    task1();
}

----------------------------------------------------------------------------

1)Definición del Tamaño del Buffer
Primero, definí el tamaño del buffer de recepción en 32 bytes utilizando una constante (BUFFER_SIZE).

2)Creación de la Tarea y Estado Inicial
Diseñé una tarea (task1) con una máquina de estados, utilizando un enum para definir los estados INIT y WAIT_DATA. En el estado INIT, la tarea inicializa el puerto serial con Serial.begin(115200).

3)Encapsulación del Buffer y Contador
Encapsulé el buffer de recepción (rxBuffer) y el contador de datos (dataCounter) dentro de la tarea como variables static. Esto asegura que los datos en el buffer y el contador no se pierdan entre las llamadas a la tarea.

4)Recepción y Almacenamiento de Datos
En el estado WAIT_DATA, la tarea verifica si hay datos disponibles en el puerto serial con Serial.available(). Si hay datos y el buffer no está lleno, lee cada byte con Serial.read() y lo almacena en rxBuffer. El contador dataCounter se incrementa para llevar la cuenta de los bytes recibidos.

5)Manejo del Buffer Lleno
Cuando el buffer se llena (es decir, dataCounter alcanza BUFFER_SIZE), se procesa el contenido del buffer (en este caso, se imprime el contenido) y luego se reinicia el contador dataCounter para reutilizar el buffer.

6)Reutilización del Buffer
Después de procesar los datos, el contador se reinicia para permitir la recepción de más datos sin perder los que ya se han recibido.

7)Ejecución Continua
Finalmente, la tarea se llama repetidamente dentro de loop() para asegurar que los datos se procesen continuamente a medida que llegan.