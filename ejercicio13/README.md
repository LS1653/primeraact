static void changeVar(uint32_t *pdata)
{
    *pdata = 10;
}

static void printVar(uint32_t value)
{
    Serial.print("var content: ");
    Serial.print(value);
    Serial.print('\n');
}

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
            uint32_t var = 0;
            uint32_t *pvar = &var;
            printVar(*pvar);
            changeVar(pvar);
            printVar(var);
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

---------------------------------------------------------------------------------------------------

Hipotesis del codigo:
Según lo que entiendo lo que sucedera es que luego de cambiar de estado de INIT a WAIT_DATA, lo que sucedera es que el valor var se imprimira siendo este de 0 y luego este valor se cambiara por el del puntero por lo que var toma el valor de 10 que estaba en el puntero y luego se imprime este valor, este proceso se repetira hasta que se detenga el programa, esa es mi ipotesis del codigo.

Resultado que daba:
Como mencione se imprimen 0 y 10, sin embargo deje pasar dos punstos claves, el primero es que tambien cada vez que se ejecute el codigo, este tambien imprimira la frase var content y lo más importante es que el programa si puede imprimir infinitamente estos mensajes, sin embargo eso solo es posible si se le sigue enviando información osea este programa puede imprimir infinitamente esos mensajes mientras haya alguien que envie información.
    