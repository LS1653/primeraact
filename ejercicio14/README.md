static void changeVar(uint32_t *pdata)
{
    *pdata = 0;    
}
static void changeVar2(uint32_t *pdata2)
{
    *pdata2 = 20;    
}


static void printVar(uint32_t value)
{
    Serial.print("var content2: ");
    Serial.print(value);
    Serial.print('\n');
}

static void printVar2(uint32_t value)
{
    Serial.print("var content: ");
    Serial.print(value);
    Serial.print('\n');
}



void tarea1()
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
            // DEBES leer ese dato, sino se acumula y el buffer de recepción 
            //del serial se llenará.   
            Serial.read();
            uint32_t var = 0;
            uint32_t var2 = 20;

            // Almacena en pvar la dirección de var.      
            uint32_t *pvar = &var;
            uint32_t *pvar2 = &var2;

            
            Serial.print("var content: ");
            changeVar(pvar);
            changeVar2(pvar2);
            printVar(var);
            printVar2(var2);
            Serial.print('\n');

           
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
    tarea1();
}

void loop()
{
    tarea1();
}