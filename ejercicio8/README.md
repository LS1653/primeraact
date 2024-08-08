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