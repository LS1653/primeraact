
void task1()
{
    // Definición de estados y variable de estado
    enum class Task1States
    {
        INIT,
        WAIT_TIMEOUT
    };
    static Task1States task1State = Task1States::INIT;

    // Definición de variables static (conservan su valor entre llamadas a task1)
    static uint32_t lastTime = 0;

    // Constantes
    constexpr uint32_t INTERVAL = 1000;

    // MÁQUINA de ESTADOS
    switch (task1State)
    {
    case Task1States::INIT:
    {
        // Acciones:
        Serial.begin(115200);

        // Garantiza los valores iniciales para el siguiente estado.
        lastTime = millis();
        task1State = Task1States::WAIT_TIMEOUT;

        Serial.print("Task1States::WAIT_TIMEOUT\n");

        break;
    }
    case Task1States::WAIT_TIMEOUT:
    {
        uint32_t currentTime = millis();

        // Evento
        if ((currentTime - lastTime) >= INTERVAL)
        {
            // Acciones:
            lastTime = currentTime;
            Serial.print(currentTime);
            Serial.print('\n');
        }
        break;
    }
    default:
    {
        Serial.println("Error");
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
-----------------------------------------------------------
¿Cómo se ejecuta este programa?
El programa en terminos simples se ejecuta en forma de un bucle generado por el llamado de funciones, para que este programa haga lo antes mencionado, lo primero que hace es darle dos estados a Task1States los cuales son INIT y WAIT_TIMEOUT, a Task1States se le da desde el inicio el estado INIT, este mismo estado es cuestionado en el condicional swicth donde si es INIT Inicializa la comunicación serie a 115200 baudios, guardara el tiempo actual en lastTime, cambia el estado a WAIT_TIMEOUT, imprime un mensaje indicando el cambio de estado; en caso de que su estado sea WAIT_TIMEOUT comprueba si ha pasado el INTERVAL desde lastTime, si ha pasado, actualiza lastTime al tiempo actual y imprime el tiempo actual, en caso de que no sea ninguno imprime un mensaje de error si se alcanza un estado no definido y ya por ultimo lo que pasa es que setup se ejecuta una vez al inicio y loop se ejecuta repetidamente. En setup se llama a task1 para inicializarla, y en loop se llama repetidamente para ejecutar la máquina de estados.

Pudiste ver este mensaje: Serial.print("Task1States::WAIT_TIMEOUT\n");. ¿Por qué crees que ocurre esto?
No pude verlo, considero yo ya que cuando activo el programa me toca volver a abrir en ocaciones el monitor serial y en ese punto cuando lo abro el progrma ya esta comensando a impreimir currentTime en más de 3000, esto se puede decir que es cierto debido a que el programa funciona bien, osea sus partes no generan un fallo en el que no pueda cmabiar de estado o se bloquee en alguna parte del programa y tampoco es un fallo en el Serial.print("Task1States::WAIT_TIMEOUT\n") ya que si se pone en el segundo case del switch si se imprime sin problemas.

¿Cuántas veces se ejecuta el código en el case Task1States::INIT?
El código dentro del case Task1States::INIT se ejecuta solo una vez durante el programa. Esto es porque una vez que se ejecuta, el estado task1State se cambia a Task1States::WAIT_TIMEOUT, lo se refiere a que en todas las llamadas subsecuentes a task1(), el programa entrará directamente en el case Task1States::WAIT_TIMEOUT del switch