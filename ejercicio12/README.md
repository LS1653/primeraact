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
            // DEBES leer ese dato, sino se acumula y el buffer de recepción 
            //del serial se llenará.   
		        Serial.read();
            uint32_t var = 0;

            // Almacena en pvar la dirección de var.      
			      uint32_t *pvar = &var;

            // Envía por el serial el contenido de var usando 
           // el apuntador pvar.    
            Serial.print("var content: ");
            Serial.print(*pvar);
            Serial.print('\n');

            // ESCRIBE el valor de var usando pvar   
            *pvar = 10;
            Serial.print("var content: ");
            Serial.print(*pvar);
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
    task1();
}

void loop()
{
    task1();
}

-------------------------------------------------------------------------------------------

La variable pvar se conoce como puntero. Simplemente es una variable en la cual se almacenan direcciones de otras variables. En este caso, en pvar se almacena la dirección de var. Nota que debes decirle al compilador el tipo de la variable (uint32_t en este caso) cuya dirección será almacenada en pvar.

¿Cómo se declara un puntero?
Un puntero se declara especificando el tipo de dato al que apuntará, seguido de un asterisco (*) y el nombre del puntero. 
Por ejemplo: int *ptr;  // Declara un puntero a un entero
En el caso del codigo que nos dio la profesora: uint32_t *pvar;  // Declara un puntero a un entero sin signo de 32 bits

¿Cómo se define un puntero? (cómo se inicializa)
Para definir (o inicializar) un puntero, se debe asignar la dirección de una variable del mismo tipo que el puntero. Esto se hace usando el operador de dirección (&). Ejemplo: int var = 5;
         int *ptr = &var;
En el caso del codigo que nos dio la profesora: pvar = &var;  // pvar ahora apunta a la dirección de la variable var

¿Cómo se obtiene la dirección de una variable?
La dirección de una variable se obtiene usando el operador de dirección (&). 
Por ejemplo: int var = 10;
             int *ptr = &var;  // Aquí obtenemos la dirección de var y la asignamos al puntero ptr
En el caso del codigo que nos dio la profesora: uint32_t *pvar = &var;  // Asignamos la dirección de var al puntero pvar

¿Cómo se puede leer el contenido de una variable por medio de un puntero?
Para leer el contenido de una variable a través de un puntero, se usa el operador de desreferencia (*). Esto permite acceder al valor almacenado en la dirección a la que apunta el puntero. 
Ejemplo: int var = 10;
         int *ptr = &var;
         int value = *ptr;  // value ahora contiene 10, el valor almacenado en var
En el caso del codigo que nos dio la profesora: Serial.print(*pvar);  // Lee y envía el valor de var usando el puntero pvar

¿Cómo se puede escribir el contenido de una variable por medio de un puntero?
Para escribir (o modificar) el contenido de una variable usando un puntero, se usa el operador de desreferencia (*) y se asigna el nuevo valor.
Ejemplo: nt var = 10;
         int *ptr = &var;
         *ptr = 20;  // Ahora var contiene 20
En el caso del codigo que nos dio la profesora: *pvar = 10;  // Modifica el valor de var a 10 usando el puntero pvar

