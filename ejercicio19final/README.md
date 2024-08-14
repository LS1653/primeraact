En una experiencia un grupo terrorista llamado “Disedentes del tiempo” realizan la configuración de una central nuclear global con la que se realizará la emisión de radiación nuclear al mundo. El sistema utiliza una interfaz de usuario simulada mediante el puerto serial, implementada en un entorno de software de Arduino.

En esta configuración inicial el sistema inicia en modo de configuración, mostrando una vez el mensaje CONFIG. En este modo se le configura el tiempo para abrir la cámara, por defecto tiene 5 seg y puede configurarse  entre 1 y 40 segundos con los botones S(Subir) y B(Bajar) en pasos de 1 segundo. Tan pronto se ajuste el tiempo de apertura de la cámara se termina con la letra L (listo) y se observa la cuenta en una pantalla, al final de la cual se abre la cámara y se hace la emisión de la radiación.  Cuando la cuenta regresiva termine debe mostrarse el mensaje “RADIACIÓN NUCLEAR ACTIVA” y a los dos segundos pasar nuevamente al modo CONFIG.

La misión del participante de la experiencia es salvar el mundo, por lo cual debe descifrar con pistas e ingresar el código de acceso, para lo cual  se envía por el puerto serial la secuencia 'C' seguido de la clave numérica (por ejemplo, 'C1234'). Si ingresas la clave correcta debe aparecer el mensaje “SALVASTE AL MUNDO”, sino la cuenta regresiva debe continuar hasta el fatal desenlace.

enum class SystemState {
    CONFIG,
    COUNTDOWN,
    RADIATION,
    SAFETY
};

SystemState currentState = SystemState::CONFIG;
const int defaultTime = 5;
const int maxTime = 40;
const int minTime = 1;
int countdownTime = defaultTime;
const String accessCode = "1234"; // Código de acceso correcto
String enteredCode = "";
unsigned long countdownStartTime = 0;

void setup() {
    Serial.begin(115200);
    Serial.println("Sistema Iniciado.");
    Serial.println('\n');
    Serial.println("Modo CONFIG: Establezca el tiempo de apertura de la cámara.");
    Serial.println('\n');
    Serial.println("Presione 'S' para subir el tiempo, 'B' para bajar el tiempo.");
    Serial.println('\n');
    Serial.println("Cuando esté listo, presione 'L' para iniciar la cuenta regresiva.");
    Serial.println('\n');
    Serial.print("Tiempo actual configurado: ");
    Serial.println('\n');
    Serial.println(countdownTime);
}

void loop() {
    switch (currentState) {
        case SystemState::CONFIG:
            handleConfig();
            break;
        case SystemState::COUNTDOWN:
            handleCountdown();
            break;
        case SystemState::RADIATION:
            handleRadiation();
            break;
        case SystemState::SAFETY:
            Serial.println("SALVASTE AL MUNDO");
            while (true); // Detener el programa
            break;
    }
}

void handleConfig() {
    if (Serial.available() > 0) {
        char input = Serial.read();
        if (input == 'S' && countdownTime < maxTime) {
            countdownTime++;
            Serial.print("Tiempo configurado: ");
            Serial.println(countdownTime);
        } else if (input == 'B' && countdownTime > minTime) {
            countdownTime--;
            Serial.print("Tiempo configurado: ");
            Serial.println(countdownTime);
        } else if (input == 'L') {
            Serial.print("Cuenta regresiva en: ");
            Serial.println(countdownTime);
            currentState = SystemState::COUNTDOWN;
            countdownStartTime = millis();
        } else {
            Serial.println("Entrada no válida. Use 'S' para subir, 'B' para bajar, 'L' para iniciar.");
        }
    }
}

void handleCountdown() {
    unsigned long currentTime = millis();
    unsigned long elapsedTime = (currentTime - countdownStartTime) / 1000;
    int remainingTime = countdownTime - elapsedTime;

    if (remainingTime > 0) {
        Serial.print("Tiempo restante: ");
        Serial.println(remainingTime);

        if (Serial.available() > 0) {
            char input = Serial.read();
            if (input == 'C') {
                enteredCode = Serial.readStringUntil('\n');
                if (enteredCode == accessCode) {
                    currentState = SystemState::SAFETY;
                } else {
                    Serial.println("Código incorrecto. Continuando cuenta regresiva.");
                }
            } else {
                Serial.println("Entrada no válida durante la cuenta regresiva. Ingrese 'C' seguido del código.");
            }
        }

        delay(1000);
    } else {
        currentState = SystemState::RADIATION;
    }
}

void handleRadiation() {
    Serial.println("RADIACIÓN NUCLEAR ACTIVA");
    delay(2000); // Esperar 2 segundos
    currentState = SystemState::CONFIG;
    Serial.println("Volviendo al modo CONFIG. Establezca el tiempo de apertura de la cámara.");
    countdownTime = defaultTime; // Reiniciar tiempo de configuración
}
