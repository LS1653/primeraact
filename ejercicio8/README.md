void Funcion()
{
    // Definición de estados y variable de estado
    enum class Estados
    {
        PRIMERO,
        CAMBIO,
        DOBLECAMBIO
    };
    static Estados estado = Estados::PRIMERO;
    static uint32_t previousTime = millis();
    uint32_t currentTime = millis();

    // MÁQUINA de ESTADOS
    switch (estado)
    {
    case Estados::PRIMERO:
    {
        if ((currentTime - previousTime) >= 1000)
        {
            Serial.print("uno");
            Serial.print('\n');
            previousTime = currentTime;
            estado = Estados::CAMBIO;
        }
        break;
    }
    case Estados::CAMBIO:
    {
        if ((currentTime - previousTime) >= 1000)
        {
            Serial.print("dos");
            Serial.print('\n');
            previousTime = currentTime;
            estado = Estados::DOBLECAMBIO;
        }        
        break;
    }
    case Estados::DOBLECAMBIO:
    {
        if ((currentTime - previousTime) >= 1000)
        {
            Serial.print("tres");
            Serial.print('\n');
            previousTime = currentTime;
            estado = Estados::PRIMERO;
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
    Serial.begin(9600);
}

void loop()
{
    Funcion();
}

---------------------------------------------------------------

¿Cuáles son los estados del programa?
Los estados del programa en este caso son tres PRIMERO, CAMBIO, DOBLECAMBIO, uno para cada mensaje que se tenia que imprimir en el Serial.

¿Cuáles son los eventos?
Los eventos de este programa son cada uno de los condicionales if que contienen los mensajes que se deben imprimir en un segundo exacto.

¿Cuáles son las acciones?
Las accines del programa son en principal medida los cambios de estado que se hacen durante el programa, tambien los print de los mensajes y las funciones como lo son el Serial.begin(9600) y millis().